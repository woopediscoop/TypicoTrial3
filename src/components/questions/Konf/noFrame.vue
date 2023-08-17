<template>
    <h3>Konfektionsart</h3>

    <button class="sel" @click="Konf({cat:'Konfektion',val:'Hohlsaum'})">Hohlsaum</button>
    <button class="sel" @click="Konf({cat:'Konfektion',val:'Klett'})">Flausch- und Klettklebestreifen</button>
    <button class="sel" @click="Konf({cat:'Konfektion',val:''})">Ohne Konfektion</button>
</template>

<script>

    export default {
        methods:{
            Konf(konfektion){
                this.$emit('final', konfektion)
            }
        }
    }

</script>