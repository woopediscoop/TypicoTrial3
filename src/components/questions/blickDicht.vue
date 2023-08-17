<template>
    <h3>Wie transparent soll das Material sein?</h3>
    <button class="sel" @click="Blickdicht({cat:'Einsatz',val:['Öffnungsanteil 15 %', 'Öffnungsanteil 8 %', 'Öffnungsanteil 35 %', 'transparent', 'lichtdurchlässig', 'Öffnungsanteil 28 %']})">Transparent</button>
    <button class="sel" @click="Blickdicht({disect:true,cat:'Einsatz',val:['Öffnungsanteil 15 %', 'Öffnungsanteil 8 %', 'Öffnungsanteil 35 %', 'transparent', 'lichtdurchlässig', 'Öffnungsanteil 28 %']})">Nicht transparent</button>
    <button class="sel" @click="Blickdicht({cat:'Einsatz',val:['absolut licht- und blickdicht', 'lichtdicht', 'absolut lichtdicht', 'blickdicht']})">Absolut licht- und blickdicht</button>
    <button class="sel" @click="Blickdicht({val:''})">Nicht wichtig</button>
</template>

<script>
    export default {
        data(){
            return {
            disabled: true,
            yes: false,
            no: false,
            }
        },
        methods: {
            Blickdicht(blickdichte){
                this.$emit('hereyago', blickdichte)
            }
        }
    }
</script>
