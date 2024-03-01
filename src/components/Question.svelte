<script>
  import { createEventDispatcher } from "svelte";

    export let question;
    export let answer;
    export let options;
    export let imageUrl;
    export let imageAltText;

    let buttonDisabled = false;

    let optionColour = "lightskyblue";

    export const toggleOption = function() {
        buttonDisabled = !buttonDisabled;
    };

    let dispatch = createEventDispatcher();
    function check(optionSelected) {
        dispatch("message", {
            optionSelected, answer
        })

        if(optionSelected !== answer) {
            optionColour = "red";
        } else {
            optionColour = "lime";
        }
    }
</script>

<div class="question__card">
    <div class="question">
        <p class="question__text">{question}</p>
        <img src={imageUrl} alt={imageAltText} width="250" height="250"/>
    </div>
    <div class="options">
        {#each options as option}
            <button on:click={() => { check(option) }} disabled={buttonDisabled} class="option">{option}</button>
        {/each}
    </div>
</div>

<style>
    .question__card {
        display: flex;
        justify-content: space-around;
        align-items: flex-start;
    }

    .question__text {
        text-align: left;
    }

    .options {
        display: flex;
        flex-direction: column;
        align-self: flex-end;
        /* display: block; */
    }

    .option {
        width: 250px;
    }

    .option.correct {
        background-color: lime;
    }

    .option.wrong {
        background-color: red;
    }

    .question {
        margin-right: 2rem;
    }

    .option {
        background-color: lightskyblue;
        margin-bottom: 1rem;
        color: white;
    }
</style>
