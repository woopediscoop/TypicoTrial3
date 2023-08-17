<template>
    <h3>Wird das Element hinterleuchtet?</h3>
    <button class="sel" @click="Backlit({cat:'Einsatz', val:'hinterleuchtbar'})">Ja</button>
    <button class="sel" @click="Backlit({val:''})">Nein</button>
</template>

<script>
    export default {
        methods: {
            Backlit(backlit){
                this.$emit('hereyago', backlit)
            }
        }
    }
</script>
