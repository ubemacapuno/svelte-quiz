<script>
    // question object will be EXPORTED from this Question.svelte component as a PROP
    export let question; 

    let answers = question.incorrect_answers;
</script>
<!-- question object will be EXPORTED from this Question.svelte component as a PROP, --> 
<h3>{@html question.question}</h3> 

{#each answers as answer}
    <button>{@html answer}</button>
{/each}



