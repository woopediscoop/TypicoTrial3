<template>
    
    <h3>Lassen Sie sich von uns kontaktieren!</h3>
    <p>{{msg}} </p>
    <p>Hinterlassen Sie Ihre Kontaktdaten, wir werden Sie schnellstmöglich kontaktieren und persönlich beraten.</p>
    <label for="tel">Tel. Nr.:</label>
    <input type="tel" id="tel">
    <label for="mail">Email:</label>
    <input type="email" id="mail">
    <button>Fertig</button>

</template>

<script>

    export default{
        props: ['msg']
    }

</script>
