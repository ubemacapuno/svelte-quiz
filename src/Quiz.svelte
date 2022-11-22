<script>
    import Question from "./Question.svelte"


    let activeQuestion = 0;
    let score = 0;
    let quiz = getQuiz();  //quiz is a promise object from the API. We have to await this since it is going to be asynchronous

    //Async function to grab a quiz from the API
    async function getQuiz() {
        const res = await fetch('https://opentdb.com/api.php?amount=10&type=multiple')
        const quiz = await res.json();
        return quiz;
    }

    //Function to get a new quiz


    //Function to advance to the next question! That way you can only answer ONCE:
    function nextQuestion() {
        activeQuestion = activeQuestion + 1;

    }

    // Function to reset the score to 0:
    function resetQuiz() {
        score = 0;
        quiz = getQuiz()
    }

    //Function to add 1 to the score:
    function addToScore() {
        score++; 

    }


</script>

<div>
    <button on:click={resetQuiz}>Fetch a New Quiz</button> 

    <h3>My Score: {score}</h3>
    <h4>Question #{activeQuestion + 1} </h4>

    {#await quiz}
        <h3 class="loading">Loading . . .</h3>
    {:then data} 

        {#each data.results as question, index}
            {#if index === activeQuestion}
                <Question {addToScore}{nextQuestion}{question} />
            {/if}
        {/each}
    {/await}

</div>

<style>
    h3{
        color: coral;
    }

    .loading{
        color: cyan;
    }

</style>