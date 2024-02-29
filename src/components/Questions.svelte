<script>
    import Question from "./Question.svelte";

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

    function gotoNextQuestion() {
        currentQuestion++;
        questionComponent.toggleOption();
    }

    function checkAnswer(event) {
        if(event.detail.optionSelected === event.detail.answer) {
            score++;
        }
        questionComponent.toggleOption();
    }
    
    
    function getQuestions() {
        return fetch('src/assets/data/questions.json').then(response => response.json());
    }
</script>

<div>
    <div class="question__details">
        <p>Question {currentQuestion}/{NUMBER_OF_QUESTIONS}</p>
        <p>Score: {score} points</p>
    </div>
    {#await getQuestions()}
        <p>Loading...</p>
    {:then quizQuestions} 
        <Question on:message={checkAnswer} bind:this={questionComponent} {...quizQuestions[questionIndexes[currentQuestion - 1]]}/>
    {/await}
    {#if currentQuestion < NUMBER_OF_QUESTIONS}
        <button on:click={gotoNextQuestion} class="green__button">Next</button>
    {/if}
</div>

<style>
    .question__details {
        display: flex;
        justify-content: space-between;
    }
    .green__button {
        background-color: limegreen;
        color: white;
    }
</style>
