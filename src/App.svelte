<script>
import { _, isLoading, locale } from "svelte-i18n"; 
import QuizContainer from "./components/QuizContainer.svelte";
import LocaleSwitcher from "./components/LocaleSwitcher.svelte";

let gameStarted = false;
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
  <QuizContainer/>
  {:else}
    <button on:click={() => gameStarted = true}  id="start__button">{$_('start_button')}</button>
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
  #start__button {
    font-size: 1.25rem;
    background-color: limegreen;
    color: white;
  }
</style>
