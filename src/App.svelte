<script>
import { _, isLoading, locale } from "svelte-i18n"; 
import Questions from "./components/Questions.svelte";
import LocaleSwitcher from "./components/LocaleSwitcher.svelte";

let gameStarted = false;
</script>

{#if $isLoading}
  <p>Wait</p>
{:else}
<header>
  <p id="title">{$_('title')}</p>
  <LocaleSwitcher currentLocale={$locale}/>
</header>
<main>
  {#if gameStarted}
  <Questions/>
  {:else}
    <button on:click={() => gameStarted = true}  id="start__button">{$_('start_button')}</button>
  {/if}
</main>
{/if}
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
