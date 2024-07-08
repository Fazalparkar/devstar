<script lang="ts">
    import { writable } from "svelte/store";
    import jwt_decode from "jwt-decode";
    
    let inputJWT = "";
    let decodedJWT: any = null;
    let errorMessage = writable("");
    let expirationMessage = writable("");

    function decodeJWT() {
        try {
            const [, payloadBase64] = inputJWT.split(".");
            const decodedPayload = JSON.parse(atob(payloadBase64.replace(/-/g, '+').replace(/_/g, '/')));
            decodedJWT = decodedPayload;
            errorMessage.set("");

            // Check expiration
            const currentTime = Math.floor(Date.now() / 1000);
            if (decodedJWT.exp && decodedJWT.exp < currentTime) {
                expirationMessage.set("Warning: The JWT token is expired.");
            } else {
                expirationMessage.set("");
            }
        } catch (error) {
            decodedJWT = null;
            errorMessage.set("Error decoding JWT. Please check your input.");
            expirationMessage.set("");
        }
    }

    function clearFields() {
        inputJWT = "";
        decodedJWT = null;
        errorMessage.set("");
        expirationMessage.set("");
    }

    async function pasteFromClipboard() {
        try {
            inputJWT = await navigator.clipboard.readText();
            decodeJWT();
        } catch (error) {
            console.error('Failed to read from clipboard:', error);
        }
    }
</script>

<div class="card gap-16 items-center mx-auto max-w-screen-xl lg:grid lg:grid-cols-2 overflow-hidden rounded-lg">
    <!-- Add tool here -->
    <section class="bg-white dark:bg-gray-900">
        <div class="py-8 px-4 mx-auto max-w-screen-xl lg:px-12">
            <div class="card p-8 relative items-center mx-auto max-w-screen-xl overflow-hidden rounded-lg">
                <div class="items-center mx-auto max-w-screen-xl overflow-hidden">
                    <div class="rounded-lg overflow-hidden bg-gray-50 border border-gray-300 mb-4">
                        <textarea
                            class="input block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                            placeholder="Enter your JWT here"
                            bind:value={inputJWT}
                            rows="4"
                        ></textarea>
                    </div>

                    {#if $errorMessage}
                        <div class="mt-4 text-red-500">{$errorMessage}</div>
                    {/if}

                    {#if $expirationMessage}
                        <div class="mt-4 text-yellow-500">{$expirationMessage}</div>
                    {/if}

                    <div class="rounded-lg overflow-hidden bg-gray-50 border border-gray-300 mt-4">
                        <textarea
                            class="output block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                            placeholder="Decoded JWT will appear here"
                            value={decodedJWT ? JSON.stringify(decodedJWT, null, 2) : ""}
                            rows="8"
                            readonly
                        ></textarea>
                    </div>

                    <div class="buttons">
                        <button class="decode-btn" on:click={decodeJWT}>Decode JWT</button>
                        <button class="clear-btn" on:click={clearFields}>Clear</button>
                        <button class="paste-btn" on:click={pasteFromClipboard}>Paste from Clipboard</button>
                    </div>
                </div>
            </div>
        </div>
    </section>
</div>

<style>
    .card {
        border: 1px solid #ddd;
        padding: 16px;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }

    .input, .output {
        width: 100%;
        padding: 8px;
        border: 1px solid #ddd;
        border-radius: 4px;
        margin-bottom: 8px;
    }

    .buttons {
        display: flex;
        justify-content: space-between;
        margin-bottom: 16px;
    }

    button {
        padding: 8px 16px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }

    .decode-btn {
        background-color: #007bff;
        color: white;
    }

    .clear-btn {
        background-color: #dc3545;
        color: white;
    }

    .paste-btn {
        background-color: #28a745;
        color: white;
    }

    @media (max-width: 600px) {
        .buttons {
            flex-direction: column;
        }

        button {
            margin-bottom: 8px;
            width: 100%;
        }
    }
</style>
