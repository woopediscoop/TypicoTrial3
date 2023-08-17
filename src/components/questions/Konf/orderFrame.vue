<template>
   <h3>Lassen Sie sich von uns kontaktieren!</h3>
    <p>Unser Bestellsystem ist nicht geeignet um ein Rahmensystem mitzubestellen. </p>
    <p>Hinterlassen Sie Ihre Kontaktdaten, wir werden Sie schnellstmöglich kontaktieren und persönlich beraten.</p>
    <label for="tel">Tel. Nr.:</label>
    <input type="tel" id="tel">
    <label for="mail">Email:</label>
    <input type="email" id="mail">
    <button>Fertig</button>
</template>

<script>
    export default{
        data(){
            return{
                ContactForm: null,
            }
            
        },
       /*  methods:{
            renderCF: () => {import('../../pages/ContactForm.vue');}
        },
        created(){
            this.renderCF().then((module) => {
                this.ContactForm = module.default;
            })
        } */


    }
</script>
