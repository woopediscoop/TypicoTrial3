<template>

    <h3>Wird der Print witterungsgeschützt sein? (Überdacht, etc.)?</h3>
    <button @click="Weather({cat:'Einsatz', val:['Outdoor','Weathered'], first:true})" class="sel">Ja</button>
    <button @click="Weather({cat:'Einsatz', val:'Outdoor', first:true})" class="sel">Nein</button>
    
</template>

<script>
    export default {
        methods: {
            Weather(weathered){
                this.$emit('final', weathered);
            }
        }
    }
</script>
