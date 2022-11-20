<script>
    import Question from "./Question.svelte"
    // export let quizName = "Corey Quiz" //use let (not const!) if you're planning to make quizName reassignable

    // let title = "TITLE";
    // let a = 1;
    // let b = 2;
    // let result = "";
    // let correctAnswer = "b"
    // let answers = ["a", "b", "c", "d"]
    let quiz = getQuiz();  //quiz is a promise object from the API. We have to await this since it is going to be asynchronous

    // function pickAnswer(answer){
    //     if (answer === correctAnswer){
    //         return result = "Correct!"
    //     }
    //     result = "OOPS"
    // }

    async function getQuiz() {
        const res = await fetch('https://opentdb.com/api.php?amount=10&type=multiple')
        const quiz = await res.json();
        return quiz;
    }

    function handleClick() {
        quiz = getQuiz();
    }


</script>

<div>
    <!-- <h2>{quizName}:</h2> -->
    <!-- <h4>{title}</h4>
    <input bind:value={title} type="text" /> -->
    <!-- <input type="number" bind:value={a}>
    <input type="number" bind:value={b}>
    <h4>{a + b}</h4> -->
    <button on:click={handleClick}>Fetch Quiz</button> 
    <!-- on:click should trigger handleClick, that way everytime it clicks, it can update with the new trivia question. -->

    <!-- {#if result}
        <h4>{result}</h4>
    {:else}
        <h5>Please pick an answer.</h5>
    {/if} -->

    <!-- without await, results[0] will be undefined. We need to await the promise! -->
    {#await quiz}
        <h3 class="loading">loading . . .</h3>
    {:then data} 
        {#each data.results as question}
            <Question {question} />
        {/each}
    {/await}
    <!-- <button on:click={() => pickAnswer("a")}>Answer A</button>
    <button on:click={() => pickAnswer("b")}>Answer B</button>
    <button on:click={() => pickAnswer("c")}>Answer C</button>
    <button on:click={() => pickAnswer("d")}>Answer D</button> -->
</div>

<style>
    h3{
        color: coral;
    }

    .loading{
        color: cyan;
    }

</style>