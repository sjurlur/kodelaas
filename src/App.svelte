<script>
  import { fade } from "svelte/transition";
  const buttons = [1, 2, 3, 4, 5, 6, 7, 8, 9, "𐄂", 0, "✓"];
  let userInput = "";
  let code = "9876";

  let error = false;
  let solved = false;

  function click(value) {
    error = false;
    requestAnimationFrame(() => {});
    if (value === "𐄂") {
      clear();
    } else if (value === "✓") {
      submit(userInput);
    } else {
      userInput += value;
    }
  }

  function clear() {
    userInput = "";
  }

  function submit(userInput) {
    if (userInput === code) {
      solved = true;
    } else {
      error = true;
      setTimeout(() => {
        clear();
        error = false;
      }, 500);
    }
  }
</script>

<main>
  {#if !solved}
    <div out:fade>
      <code>{userInput}</code>
      <div class="buttons">
        {#each buttons as button, i}
          {#if i === buttons.length - 1}
            <button
              class:error
              class:solved
              class="button"
              on:click={() => {
                click(button);
              }}>{button}</button
            >
          {:else}
            <button
              class="button"
              on:click={() => {
                click(button);
              }}>{button}</button
            >
          {/if}
        {/each}
      </div>
    </div>
  {:else}
    <div class="answer" in:fade={{ delay: 350 }}>
      <h2>Hipp hipp hurra! 🎉</h2>
      <p>Neste hint er i en maskin som har samme initialer som Hans Petter</p>
    </div>
  {/if}
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
  }

  code {
    flex: 1 1 auto;
    display: block;
    margin-bottom: 1rem;
    min-height: 27px;
    font-size: 1rem;
  }

  .buttons {
    display: grid;
    gap: 20px;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
  }

  .button {
    padding: 0.5rem 0.75rem;
    font-size: 1.5rem;
    font-weight: 500;
    background-color: rgba(233, 233, 237, 1);
  }

  .error {
    animation: fadered 0.2s forwards ease-out;
    animation-delay: 0.3s;
    background-color: rgba(169, 0, 0, 1);
  }

  .solved {
    animation: fadered 0.3s forwards ease-out;
    animation-delay: 1s;
    background-color: rgba(0, 169, 0, 1);
  }

  @keyframes fadered {
    from {
      background-color: rgba(169, 0, 0, 1);
    }
    to {
      background-color: rgba(233, 233, 237, 1);
    }
  }
  @keyframes fadegreen {
    from {
      background-color: rgba(0, 169, 0, 1);
    }
    to {
      background-color: rgba(233, 233, 237, 1);
    }
  }

  .answer {
    max-width: 400px;
    font-size: 2rem;
  }
</style>
