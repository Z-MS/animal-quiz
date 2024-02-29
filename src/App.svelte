<script>
import Questions from "./components/Questions.svelte";
let gameStarted = false;
</script>

<header>
  <p id="title">Animals Quiz</p>
</header>
<main>
  {#if gameStarted}
  <Questions/>
  {:else}
    <button on:click={() => gameStarted = true}  id="start__button">Start</button>
  {/if}
</main>

<style>
  #title {
    margin: 0;
    font-size: large;
  }
  #start__button {
    background-color: limegreen;
    color: white;
  }
</style>
