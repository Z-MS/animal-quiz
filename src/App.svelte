<script>
import { _, isLoading, locale } from "svelte-i18n"; 
import QuizContainer from "./components/QuizContainer.svelte";
import LocaleSwitcher from "./components/LocaleSwitcher.svelte";

let isFirstGame = true;
let gameStarted = false;
let gameEnded = false;
let score = 0;

function startGame() {
  gameStarted = true;
  gameEnded = false;
  isFirstGame = false;
}

function endGame(event) {
  score = event.detail.score;
  gameStarted = false;
  gameEnded = true;
}
</script>

{#if $isLoading}
  <p>Wait</p>
{:else}
<header>
  <p id="title">{$_('title')}</p>
</header>
  {#if !gameStarted}
    <div>
      <LocaleSwitcher currentLocale={$locale}/>
    </div>
  {/if}
<main>
  {#if gameStarted}
  <QuizContainer on:game-over={endGame}/>
  {:else}
    <button on:click={startGame} id="start__button">
      {#if isFirstGame}
        {$_('start_button')}
        {:else} 
          {$_('restart_button')}
      {/if}
    </button>
  {/if}

  {#if gameEnded}
    <div id="game__over">
      <p>{$_('game_over')}</p>
      <p>{$_('score', { values: { score } })}</p>
      <button>Restart</button>
    </div>
  {/if}
</main>
{/if}
<style>

  main {
    margin-top: 10%;
  }

  #title {
    margin: 0 1rem 1rem 0;
    font-size: 2rem;
  }

  #game__over {
    border: 0.5px solid gray;
  }

  #start__button {
    font-size: 1.25rem;
    background-color: limegreen;
    color: white;
  }
</style>
