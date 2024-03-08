<script>
  import { createEventDispatcher } from "svelte";

    export let question;
    export let answer;
    export let options;
    export let imageUrl;
    export let imageAltText;

    let buttonDisabled = false;
    let optionSelected = null;

    export const enableOptionButtons = function() {
        buttonDisabled = false;
        optionSelected = null;
    };
    
    export const disableOptionButtons = function () {
        buttonDisabled = true;
    }


    let dispatch = createEventDispatcher();
    function check(option) {
        optionSelected = option;
        dispatch("message", {
            option, answer
        })

        buttonDisabled = true;
    }
</script>

<div class="question__card">
    <div class="question">
        <p class="question__text">{question}</p>
        <img src={imageUrl} alt={imageAltText} width="250" height="250"/>
    </div>
    <div class="options">
        {#each options as option}
            <button 
                on:click={() => { check(option) }} 
                disabled={buttonDisabled} 
                class="option" 
                class:correct={ optionSelected && option === answer } 
                class:wrong={ (option === optionSelected) && (option !== answer) }>
                {option}
            </button>
        {/each}
    </div>
</div>

<style>
    .question__card {
        display: flex;
        justify-content: space-around;
        align-items: flex-start;
        flex-wrap: wrap;
        text-align: left;
    }

    .question__text {
        font-size: 1.25rem;
    }

    .options {
        display: flex;
        flex-direction: column;
        align-self: flex-end;
    }

    .option.correct {
        background-color: #19dd19;
    }
    
    .option.wrong {
        background-color: red;
    }
    
    .question {
        margin-right: 2rem;
    }
    
    .option {
        width: 250px;
        background-color: #765ad5;
        margin-bottom: 1rem;
        color: white;
    }
</style>
