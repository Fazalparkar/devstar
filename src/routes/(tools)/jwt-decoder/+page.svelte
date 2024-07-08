<script lang="ts">
    import { writable } from "svelte/store";
    let inputJWT = "";
    let decodedJWT: any = null;
    let errorMessage = writable("");
  
    function decodeJWT() {
      try {
        const [, payloadBase64] = inputJWT.split(".");
        const decodedPayload = JSON.parse(atob(payloadBase64));
  
        decodedJWT = decodedPayload;
        errorMessage.set("");
      } catch (error) {
        decodedJWT = null;
        errorMessage.set("Error decoding JWT. Please check your input.");
      }
    }
  
    function clearFields() {
      
    }
  </script>
  
  