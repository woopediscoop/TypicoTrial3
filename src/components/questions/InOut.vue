<template>
    <h3>Indoor oder Outdoor?</h3>
    <button class="sel" @click="Umgebung({cat:'Einsatz',val:'Indoor',first:true,disect:false})">Indoor</button>
    <button class="sel" @click="OutdoorSel">Outdoor</button>
</template>

<script>
    export default {
        methods: {
            Umgebung(umgebung){
                this.$emit('final', umgebung);
            },
            OutdoorSel(){
                this.$emit('continue', {lastPage:'doorProps'});
            }
        }
    }
</script>
