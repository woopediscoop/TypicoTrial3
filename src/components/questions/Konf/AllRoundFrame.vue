<template>
    <h3>Welche Rahmenart?</h3>
    <button class="sel" @click="Frame({cat:'Konfektion',val:'Gummilippe', description:'Gummilippe rundum'})">Gummilippenrahmen</button>
    <button class="sel" @click="Frame({cat:'Konfektion',val:'Kederschlaufe', description:'Kederschlaufe rundum'})">Kederschienenrahmen</button>
    <button class="sel" @click="Frame({cat:'Konfektion',val:'Ösen', description:'Ösen rundum'})">Rohrrahmen (Ösen)</button>
</template>

<script>

    export default {
        methods:{
            Frame(frame){
                this.$emit('final', frame)
            }
        },

    }

</script>
