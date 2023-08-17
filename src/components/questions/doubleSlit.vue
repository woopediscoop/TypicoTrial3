<template>
    <h3>Beidseitiger Druck?</h3>
    <button class="sel" @click="Beidseitig({cat:'Einsatz',val:'beidseitig bedruckbar'})">Ja</button>
    <button class="sel" @click="Beidseitig({val:''})">Nein</button>
</template>

<script>
    export default {
        methods: {
            Beidseitig(beidseitig){
                this.$emit('hereyago', beidseitig)
            }
        }
    }
</script>
