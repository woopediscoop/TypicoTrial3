<template>
    <h3>File-Upload</h3>
    <p>Bitte lesen Sie sich die genauen Anforderungen unseres Datenblattes durch: <a href="https://www.typico.com/wp-content/uploads/2023/07/TYPICO-Datenblatt-D.pdf" target="_blank">Datenblatt</a> </p>
    <p>Achtung: Befolgen Sie den Anweisungen genau. Ihre Daten werden nicht weiter kontrolliert.</p>

    <div v-if="!one">
        <p>Datei für die Vorderseite: </p>  
        <input type="file">
        <br>
        <p>Datei für die Rückseite: </p>
        <input type="file">
    </div>

    <div v-if="one">
        <p>Ihre Datei: </p>
        <input type="file">
    </div>
    
</template>

<script>

export default {
    props : ['both'],
    data(){
        return {
            one: true,
        }
    },
    methods:{

    },
    created(){
        if(this.both.val == "beidseitig bedruckbar"){
            this.one = false;
        }
    }
}

</script>
