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
    <div id="passowrd">
      <label for="password-length">Password Length</label>
      <input id="password-length" type="text" bind:value={lenght} />
    </div>

    {#each characters as { name, use, chars }}
      <Checkbox bind:apply={use} bind:name bind:chars />
    {/each}

    <button id="generate-password" on:click={() => generatePassword(lenght)}>
      Generate Password
    </button>
  </article>

  <article>
    <p id="password-container" on:click={copyToClipboard}>{password}</p>
    <p>{passwordCopied ? "Copied!" : ""}</p>
  </article>
</main>

<style>
  main {
    font-family: "Hind Siliguri", sans-serif;
  }
  main,
  article {
    border: 1px solid #ccc;
    padding: 1em;
    margin: 1em;
    border: none;
  }
  article {
    box-shadow: 1px 0px 20px 0px #e8e8e8;
    border-radius: 7px;
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

  div#passowrd {
    display: grid;
    grid-template-columns: 140px 10px 500px;
    grid-gap: 8px;
    margin: 16px;
  }
  /* the input should be in the second column */
  #password-length {
    grid-column: 2;
    width: 200px;
  }

  button#generate-password {
    width: 500px;
    height: 50px;
    background-color: rgb(100, 149, 237);
    color: white;
    font-size: x-large;
    box-shadow: 1px 0px 20px 0px #e8e8e8;
    border-radius: 7px;
    border: none ;
  }

  button#generate-password:active {
    box-shadow: none;
    background-color: rgb(100, 145, 237);

  }

</style>
