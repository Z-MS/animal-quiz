<script>
    import { _, locale } from "svelte-i18n";
    import Question from "./Question.svelte";
    import { createEventDispatcher } from "svelte";

    let questionComponent;

    const NUMBER_OF_QUESTIONS = 5;

    function pickRandomIndexes() {
        const min = 0;
        const max = 9;
        let randomIndexes = [];

        while(randomIndexes.length < NUMBER_OF_QUESTIONS) {
            const randomIndex = Math.floor(Math.random() * (max - min + 1) + min);
            if(!randomIndexes.includes(randomIndex)) {
                randomIndexes.push(randomIndex);
            }
        }
        return randomIndexes;
    }

    let questionIndexes = pickRandomIndexes();
    let currentQuestion = 1;
    let score = 0;
    let nextButtonDisabled = true;
    let dispatch = createEventDispatcher();

    function gotoNextQuestion() {
        currentQuestion++;
        nextButtonDisabled = true;
        questionComponent.enableOptionButtons();
    }

    function checkAnswer(event) {
        if(event.detail.option === event.detail.answer) {
            score++;
        }

        if(currentQuestion === NUMBER_OF_QUESTIONS) {
            dispatch("game-over", {
                score
            });
            return;
        }

        nextButtonDisabled = false;
        questionComponent.disableOptionButtons();
    }
    
    function getQuestions() {
        let currentLocale = $locale;
        return fetch(`src/assets/data/questions.${currentLocale}.json`).then(response => response.json());
    }
</script>

<div>
    <div class="question__details">
        <p>{$_('question_counter', 
        { 
            values: {
                current_question: currentQuestion,
                number_of_questions: NUMBER_OF_QUESTIONS
            }
        })}</p>
        <p>{$_('score', { values: { score } })}</p>
    </div>
    {#await getQuestions()}
        <p>Loading...</p>
    {:then quizQuestions} 
        <Question on:message={checkAnswer} bind:this={questionComponent} {...quizQuestions[questionIndexes[currentQuestion - 1]]}/>
    {/await}
    {#if currentQuestion < NUMBER_OF_QUESTIONS}
        <button on:click={gotoNextQuestion} id="next__button" disabled={nextButtonDisabled}>{$_('next_button')}</button>
    {/if}
</div>

<style>
    .question__details {
        display: flex;
        justify-content: space-between;
        font-weight: 700;
        color: gray;
    }
    #next__button {
        margin-top: 1rem;
        background-color: limegreen;
        color: white;
    }

    #next__button:disabled {
        background-color: rgb(89, 151, 89);
    }
</style>
