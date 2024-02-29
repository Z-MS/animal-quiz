<script>
import Questions from "./components/Questions.svelte";
let gameStarted = false;
</script>

<header>
  Animals Quiz
</header>
<main>
  {#if gameStarted}
  <Questions/>
  {:else}
    <button on:click={() => gameStarted = true}  class="start__button">Start</button>
  {/if}
</main>

<style>
  .start__button {
    background-color: limegreen;
    color: white;
  }
</style>
