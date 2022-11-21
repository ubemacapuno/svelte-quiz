<script>
    // question object will be EXPORTED from this Question.svelte component as a PROP
    export let question; 

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
        isAnswered = true;
        isCorrect = correct
    }

</script>
<!-- question object will be EXPORTED from this Question.svelte component as a PROP, --> 
<h3>
    {@html question.question}
</h3> 

<!-- If the question is answered, show this. Otherwise DON'T show: -->
{#if isAnswered}
<h4>
    {#if isCorrect}
    You are CORRECT!
    {:else}
    Answer INCORRECT!
    {/if}
</h4>
{/if}

{#each allAnswers as answer}
    <button on:click={() => checkQuestion(answer.correct)}>
        <!-- answer.answer meaning the answer object and the (dot).answer property of answer -->
        {@html answer.answer}

        <!-- Debug for showing the answer -->
        <!-- {answer.correct} -->
    </button>
{/each}



