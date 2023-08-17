<template>
    <div class="questionaire">
        <p>{{ current_question }} </p>
    </div>
</template>

<script>

export default ({

    name: "IcoTest",

    data() {
        
        let question = [];
        question[0] = "Größe und Breite bitte";

        return {
            current_question: question[0],
        }
        
    },
})
</script>
