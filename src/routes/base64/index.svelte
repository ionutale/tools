<script>
  import buffer from 'buffer';
  // Define the string
  let encodedValue = '';
  let decodedValue = '';


  function decodeBase64(val) {
    encodedValue = atob(val.currentTarget.value)
  }

  function encodeBase64(val) {
    decodedValue = btoa(val.currentTarget.value)
  }

</script>

<main>
  <div>
    <label for="base64">Base 64</label>
    <textarea class="decode" bind:value={decodedValue} on:input={(val) => decodeBase64(val)}/>
  </div>
  <div>
    <label for="text">Text</label>  
    <textarea class="encode" bind:value={encodedValue} on:input={(val) => encodeBase64(val)}/>
  </div>
</main>

<style>
  main {
    display: flex;
    flex-direction: row;
  }

  textarea {
    width: 50vw;
    height: 80vh;
  }
</style>
