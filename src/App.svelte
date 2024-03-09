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
  {#if gameEnded}
    <div id="game__over">
      <p id="game__over__text">{$_('game_over')}</p>
      <p>{$_('score', { values: { score } })}</p>
    </div>
  {/if}

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
    margin-bottom: 1rem;
    border: 0.025px solid rgb(223, 223, 223);
    border-radius: 1rem;
    box-shadow: 1px 1px 1px 0.1px rgba(0, 0, 0, 0.2);
  }

  #game__over__text {
    font-weight: 600;
  }

  #start__button {
    font-size: 1.25rem;
    background-color: limegreen;
    color: white;
  }
</style>
