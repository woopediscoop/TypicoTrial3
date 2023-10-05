<template>
    <h3>Bestellungszusammenfassung</h3>
    <p>{{ material }}</p>
    <p>Breite: {{ Selection.Width }}mm</p>
    <p>Höhe: {{ Selection.Height }}mm</p>
    <p>{{ Selection.Zugabe }}</p>
    <p>{{ Selection.Environment }}</p>
    <p>Konfektion: {{ Selection.Konfektion }}</p>
    <p v-if="KonfSpec != ''">{{ Selection.KonfSpec }}</p>
    <p v-if="KonfPlace != ''">{{ Selection.KonfPlace }}</p>
</template>

<script>

    export default {
        props:['material','qDict', 'qs'],
        data(){
            return {
                Selection: {
                    Material:"",
                    Width:"",
                    Height:"",
                    Zugabe:"",
                    Environment:"",
                    Konfektion:"",
                    KonfSpec:"",
                    KonfPlace:"",

                },
                selects:[]
            }
        },
        created(){
            this.Selection.Material = this.material;
            this.Selection.Width = this.qDict.Dimensions.value.width;
            this.Selection.Height = this.qDict.Dimensions.value.height;
            if(this.qDict.Konf.value.Zugabe){
                this.Selection.Zugabe = "Zugabe inkludiert";
            }
            if(this.qDict.InOut.value.val == ['Outdoor', 'Weathered']){
                this.Selection.Environment = "Outdoor, Überdacht";
            }
            else{
                this.Selection.Environment = this.qDict.InOut.value.val;
            }
            this.Selection.Konfektion = this.qDict.Konf.value.val;
            if(this.qDict.Konf.value.Spezifikation){
                this.Selection.KonfSpec = this.qDict.Konf.value.Spezifikation;
            }
            if(this.qDict.Konf.value.Platzierung){
                this.Selection.KonfPlace = this.qDict.Konf.value.Platzierung;
            }
            Object.keys(this.Selection).forEach(sel => {
                this.selects.push(sel);
            })


        }
    }

</script>