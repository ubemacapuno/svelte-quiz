<script>
    // question object will be EXPORTED from this Question.svelte component as a PROP
    export let question; 
    export let nextQuestion;
    export let addToScore;

    let isCorrect;
    let isAnswered = false;
    let answers = question.incorrect_answers.map(answer => {
        return {
            answer,
            correct: false
        }
    });
    let allAnswers = [
        ...answers,
        {
            answer: question.correct_answer,
            correct: true,
        }
    ];
    shuffle(allAnswers);

    //random value between -0.5 to +0.5
    function shuffle(array) {
        array.sort(()=> Math.random() - 0.5)
    }


    //Function for checking if selection "isCorrect" is correct
    function checkQuestion(correct){
        // set the value of this to the paramater "correct", otherwise it won't change when clicking the right answer:
        if(!isAnswered){
            isAnswered = true;
            isCorrect = correct;
            if(correct){
                addToScore();
            }
        }
    }
</script>

<!-- question object will be EXPORTED from this Question.svelte component as a PROP, --> 
<h3>
    {@html question.question}
</h3> 

<!-- If the question is answered, show this. Otherwise DON'T show: -->
{#if isAnswered}
    <!-- If isCorrect is true, the class of the h5 element will be "isCorrect" -->
    <h5 class:isCorrect>
        {#if isCorrect}
        You are CORRECT!
        {:else}
        Answer INCORRECT!
        {/if}
    </h5>
{/if}

{#each allAnswers as answer}
    <button class="answer" on:click={() => checkQuestion(answer.correct)}>
        <!-- answer.answer meaning the answer object and the (dot).answer property of answer -->
        {@html answer.answer}

        <!-- Debug for showing the answer -->
        <!-- {answer.correct} -->
    </button>
{/each}

<!-- Conditional to advance to the next question if the question has been answered: -->
{#if isAnswered}
    <div>
        <button on:click={nextQuestion}>Next Question</button>
    </div>
{/if}

<style>
    h5 {
        color: maroon;
    }

    .isCorrect {
        color: teal;
    }

    .answer{
        display: block;
        color: coral;
        background-color: white;
        border-radius: 10px;
        transition: 0.2s;
        min-width: 3rem;
    }

    .answer:hover {
        cursor: pointer;
        color: teal;
    }
</style>

