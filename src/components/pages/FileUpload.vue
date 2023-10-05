<template>
    <h3>File-Upload</h3>
    <p>Bitte lesen Sie sich die genauen Anforderungen unseres Datenblattes durch: <a href="https://www.typico.com/wp-content/uploads/2023/07/TYPICO-Datenblatt-D.pdf" target="_blank">Datenblatt</a> </p>
    <p>Achtung: Befolgen Sie den Anweisungen genau. Ihre Daten werden nicht weiter kontrolliert.</p>

    <div>
        <p>Ihre Datei: </p>
        <input @change="fileUpload" type="file">
        <br>
        <br>
        <button @click="Continue" :disabled="prohibitProceed">Weiter</button>
    </div>
    
</template>

<script>

export default {
    //props : ['both'],
    data(){
        return {
            prohibitProceed:true,
        }
    },
    methods:{
        fileUpload(){
            this.prohibitProceed = false;
        },
        Continue(){
            this.$emit('uploaded')
        }

    },
}

</script>
