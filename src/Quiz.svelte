<script>
    import { fly } from 'svelte/transition';
    // import { onMount, beforeUpdate, afterUpdate, onDestroy } from 'svelte'
    import Question from "./Question.svelte";
    import Modal from "./Modal.svelte"

    let activeQuestion = 0;
    let score = 0;
    let quiz = getQuiz();  //quiz is a promise object from the API. We have to await this since it is going to be asynchronous
    let isModalOpen = false

    // onMount(() => { 
    //     console.log(`Component "QUIZ" has mounted!`)
    // })

    // beforeUpdate(() => {
    //     console.log("Before Update")
    // })

    // afterUpdate(() => {
    //     console.log("After update")
    // })

    // onDestroy(() => {
    //     console.log("ON DESTROY!")
    // })

    //Async function to grab a quiz from the API
    async function getQuiz() {
        // const res = await fetch('https://opentdb.com/api.php?amount=10&type=multiple')
        const res = await fetch('https://opentdb.com/api.php?amount=10&category=31&type=multiple')
        const quiz = await res.json();
        return quiz;
    }

    //Function to advance to the next question! That way you can only answer ONCE:
    function nextQuestion() {
        activeQuestion = activeQuestion + 1;

    }

    // Function to reset the score to 0:
    function resetQuiz() {
        isModalOpen = false;
        score = 0;  
        activeQuestion = 0;
        quiz = getQuiz()
    }

    //Function to add 1 to the score:
    function addToScore() {
        score++; 
    }

    //Reactive Statement - "$"
        //$ is a label
        //Everything after : is a statement
    $: if (score > 0){
        isModalOpen = true;
    }

    //Reactive Declaration
    $: questionNumber = activeQuestion + 1

</script>

<div>
    <button on:click = {resetQuiz}>Fetch a New Quiz</button> 

    <h3>My Score: {score}</h3>
    <h4>Question #{questionNumber} </h4>

    {#await quiz}
        <h3 class="loading">Loading . . .</h3>
    {:then data} 
        {#each data.results as question, index}
            {#if index === activeQuestion}
            <div in:fly={{x: 100}} out:fly={{x: -100}} class="fade-wrapper">
                <Question {addToScore}{nextQuestion}{question} />
            </div>
            {/if}
        {/each}
    {/await}

</div>
{#if isModalOpen}
    <!-- keep in mind that on:"close" needs to match the string in the dispatch of the event in the Modal component (which is 'close')!! -->
    <Modal on:close={resetQuiz}> 
        <h2>You WON!</h2>
        <p>Congratulations</p>
        <button on:click={resetQuiz}>Start Over</button>
    </Modal>
{/if}

<style>
    h3{
        color: teal;
    }

    .loading{
        color: cyan;
    }

    .fade-wrapper {
        position: absolute;
    }
</style>