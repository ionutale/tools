<script>
  import { onMount } from "svelte";

  import Checkbox from "$lib/checkbox/index.svelte";

  let password = "";
  let lenght = 16;
  let passwordCopied = false;

  let characters = [
    {
      name: "symbols",
      use: true,
      chars: "@#!<>{}[]-_+=§$%&/()=?¿¡*|°¬",
    },
    {
      name: "numbers",
      use: true,
      chars: "1234567890",
    },
    {
      name: "lowercase",
      use: true,
      chars: "abcdefghijklmnopqrstuvwxyz",
    },
    {
      name: "uppercase",
      use: true,
      chars: "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    },
  ];

  function generatePassword(lenght) {
    let chars = "";
    for (let i = 0; i < characters.length; i++) {
      if (characters[i].use) {
        chars += characters[i].chars;
      }
    }

    password = "";
    const charsLength = chars.length;
    for (let i = 0; i < lenght; i++) {
      password += chars.charAt(Math.floor(Math.random() * charsLength));
    }
    passwordCopied = false;
  }

  function copyToClipboard() {
    // copy password to navigator clipboard
    navigator.permissions.query({ name: "clipboard-write" }).then((result) => {
      if (result.state == "granted" || result.state == "prompt") {
        /* write to the clipboard now */
        navigator.clipboard.writeText(password);
        passwordCopied = true;
      }
    });
  }

  onMount(() => {
    generatePassword(lenght);
  });
</script>

<main>
  <article>
    <div>
      <label for="password-length">Password Length</label>
      <input id="password-length" type="text" bind:value={lenght} />
    </div>

    {#each characters as { name, use, chars }}
      <Checkbox bind:apply={use} bind:name bind:chars />
    {/each}

    <button on:click={() => generatePassword(lenght)}>
      Generate Password
    </button>
  </article>

  <article>
    <p id="password-container" on:click={copyToClipboard}>{password}</p>
    <p>{passwordCopied ? "Copied!" : ""}</p>
  </article>
</main>

<style>
  main,
  article {
    border: 1px solid #ccc;
    padding: 1em;
    margin: 1em;
    border: none;
  }
  /* inline elements inside article */
  article > * {
    display: inline-block;
    margin: 0 1em;
  }
  #password-container {
    font-size: 2em;
    font-weight: bold;
    text-align: center;
    cursor: pointer;
  }
</style>
