<script lang="ts">
  import { writable } from "svelte/store";

  let inputJWT = "";
  let decodedJWT: any = null;
  let errorMessage = writable("");

  function decodeJWT() {
    // jwt logic will be added here
  }
</script>

<section class="bg-white dark:bg-gray-900">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:px-12">
    <div
      class="card p-8 relative items-center mx-auto max-w-screen-xl overflow-hidden rounded-lg"
    >
      <div class="items-center mx-auto max-w-screen-xl overflow-hidden">
        <div
          class="rounded-lg overflow-hidden bg-gray-50 border border-gray-300 mb-4"
        >
          <textarea
            placeholder="Enter your JWT here"
            bind:value={inputJWT}
            rows="4"
            class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          ></textarea>
        </div>

        {#if $errorMessage}
          <div class="mt-4 text-red-500">{$errorMessage}</div>
        {/if}

        <div
          class="rounded-lg overflow-hidden bg-gray-50 border border-gray-300 mt-4"
        >
          <textarea
            placeholder="Decoded JWT will appear here"
            value={decodedJWT ? JSON.stringify(decodedJWT, null, 2) : ""}
            rows="8"
            readonly
            class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          ></textarea>
        </div>
        <div class="mt-4">
          <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
            on:click={decodeJWT}
          >
            Decode JWT
          </button>
        </div>
      </div>
    </div>
  </div>
</section>
