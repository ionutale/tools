<script>
  export let apply = false;
  export let name = "";
  export let chars = "";
  // upper case first letter
  function ucFirst(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
  }
</script>

<div>
  <slot />
  <label for="include-symbols">Include {ucFirst(name)} </label>
  <input id="include-symbols" type="checkbox" bind:checked={apply} {apply} />
  <label for="include-symbols">{chars}} </label>
</div>

<style>
  /* the div should be a grid of 3 columns that contains the input, label, p */
  div {
    display: grid;
    grid-template-columns: 140px 10px 500px;
    grid-gap: 8px;
    margin: 16px;
  }
  /* the input should be in the second column */
  input {
    grid-column: 2;
  }
</style>
