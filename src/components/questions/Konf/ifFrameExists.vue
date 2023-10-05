<template>
    <h3>Wie soll der Print an Ihrem Rahmen befestigt werden?</h3>
    <button class="sel" @click="FrameSel('allRoundFrame')">Rundum-Lösung (gleiche Konfektion auf allen vier Seiten)</button>
    <button class="sel" @click="Construction">spezielle Konfektion (z.B. links und rechts anders als oben/unten)</button>
    <p>{{ note }}</p>
</template>

<script>

    export default {
        data(){
            return {
                note: ""
            }
        },
        methods:{
            FrameSel(frameSel){
                this.$emit('cont', {'frameStatus':frameSel, 'lastPage':'frameExists'})
            },
            Construction(){
                this.note = "Notiz: Dieser Schritt wird noch ausgebaut"
            }
            
        }
    }

</script>