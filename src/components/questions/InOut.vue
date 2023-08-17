<template>
    <h3>Indoor oder Outdoor?</h3>
    <button class="sel" @click="Umgebung({cat:'Einsatz',val:'Indoor',first:true,disect:false})">Indoor</button>
    <button class="sel" @click="Umgebung({cat:'Einsatz',val:'Outdoor',first:true,disect:false})">Outdoor</button>
</template>

<script>
    export default {
        methods: {
            Umgebung(umgebung){
                this.$emit('hereyago', umgebung)
            }
        }
    }
</script>
