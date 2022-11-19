<script>
    // export let quizName = "Corey Quiz" //use let (not const!) if you're planning to make quizName reassignable

    // let title = "TITLE";
    // let a = 1;
    // let b = 2;
    let result = "";
    let correctAnswer = "b"
    let answers = ["a", "b", "c", "d"]

    function pickAnswer(answer){
        if (answer === correctAnswer){
            return result = "Correct!"
        }
        result = "OOPS"
    }
</script>

<div>
    <!-- <h2>{quizName}:</h2> -->
    <!-- <h4>{title}</h4>
    <input bind:value={title} type="text" /> -->
    <!-- <input type="number" bind:value={a}>
    <input type="number" bind:value={b}>
    <h4>{a + b}</h4> -->
    {#if result}
        <h4>{result}</h4>
    {:else}
    <h5>Please pick an answer.</h5>
    {/if}
    {#each answers as answer}
        <button on:click={() => pickAnswer(answer)}>
            Answer {answer.toUpperCase()}
        </button>
    {/each}
    <!-- <button on:click={() => pickAnswer("a")}>Answer A</button>
    <button on:click={() => pickAnswer("b")}>Answer B</button>
    <button on:click={() => pickAnswer("c")}>Answer C</button>
    <button on:click={() => pickAnswer("d")}>Answer D</button> -->
</div>

