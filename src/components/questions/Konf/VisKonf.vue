<template>
    <h3>Konfektionskonfigurator</h3>
    
    <p>Wählen Sie Ihre gewünschte Konfektion</p>
    <select v-model="konfSel" @change="updateSelect2">
        <option value="---">---</option>
      <option v-for="option in konfList" :key="option.value" :value="option.value">
        {{ option.label }}
      </option>
    </select>

    <p v-if="second">{{ secondLabel }}</p>
    <select v-if="second" v-model="secondSel" @change="updateSelect3">
        <option key="---" value="---">---</option>
        <option v-for="option in options2" :key="option.value" :value="option.value">
            {{ option.label }}
        </option>
    </select>

    <p v-if="third">{{ thirdLabel }}</p>
    <select v-if="third" v-model="thirdSel" @change="updateContinue">
        <option value="---">---</option>
        <option v-for="option in options3" :key="option.value" :value="option.value">
            {{ option.label }}
        </option>
    </select>

    

    <button :disabled="disContinue" @click="Continue">Weiter</button>

</template>

<script>

    export default{
        props: ['konfList', 'questDict'],
        data(){
            return {
                second: false,
                secondLabel: '',
                third: false,
                thirdLabel: '',
                disContinue: true,
                options2: [],
                obenunten: '',
                linksrechts:''
            }
        },
        methods:{
            updateSelect2(){
                // an den Seiten mit Breite mm


                this.secondSel = null;
                this.third = false;
                if(this.konfSel == 'Kederschlaufe'){
                    this.second = true;
                    this.secondLabel = 'Gewünschte Größe der Kederschlaufe'
                    this.disContinue = true;
                    this.options2 = [
                        {label:'8 mm', value:'Schlaufengröße: 8 mm'},
                        {label:'10 mm', value:'Schlaufengröße: 10 mm'},
                        {label:'12 mm', value:'Schlaufengröße: 12 mm'}
                    ]
                }
                else if(this.konfSel == 'Ösen'){
                    this.second = true;
                    this.secondLabel = 'Abstand zwischen den Ösen'
                    this.disContinue = true;
                    this.options2 = [
                        {label:'alle 25cm', value:'alle 25cm'},
                        {label:'alle 50cm', value:'alle 50cm'}
                    ]
                }
                else if(this.konfSel == 'Hohlsaum'){
                    this.second = true;
                    this.secondLabel = 'Wo sollen der Hohlsaum konfektioniert sein?'
                    this.third = false;
                    this.disContinue = true;
                    this.options2 = [
                        {label:this.obenunten,value:this.obenunten},
                        {label:this.linksrechts,value:this.linksrechts}
                    ]
                }
                else if(this.konfSel == 'Gummilippe' || this.konfSel == 'Klett' || this.konfSel == ''){
                    this.second = false;
                    this.disContinue = false;
                }
                else{
                    this.second = false;
                }

                if(this.konfSel == '---'){
                    this.disContinue = true;
                }
                
            },
            updateSelect3(){
                

                this.thirdSel = null;
                if(this.konfSel == 'Ösen'){
                    this.third = true;
                    this.thirdLabel = 'Wo sollen die Ösen konfektioniert sein?'
                    this.options3 = [
                        {label:'rundum',value:'rundum'},
                        {label:this.obenunten,value:this.obenunten},
                        {label:this.linksrechts,value:this.linksrechts}
                    ]
                }
                else if (this.konfSel == 'Kederschlaufe'){
                    this.third = true;
                    this.thirdLabel = 'Wo soll die Kederschlaufe konfektioniert sein?'
                    this.options3 = [
                        {label:this.obenunten,value:this.obenunten},
                        {label:this.linksrechts,value:this.linksrechts}
                    ]
                }
                else if (this.konfSel == 'Hohlsaum'){
                    this.disContinue = false;
                }
                else{
                    this.third = false;
                }

                if (this.secondSel == "---"){
                    this.disContinue = true;
                    this.third = false;
                    this.thirdSel = null;
                }

                
            },
            
            updateContinue(){
                if(this.thirdSel == "---")
                {
                    this.disContinue = true
                }
                else{
                    this.disContinue = false;
                }
            },
            Continue(){
                if(this.konfSel == 'Gummilippe' || this.konfSel == 'Klett' || this.konfSel == ''){
                    this.$emit('cont', {cat:'Konfektion',val:this.konfSel})
                }
                else if(this.konfSel == 'Ösen' || this.konfSel == 'Kederschlaufe'){
                    this.$emit('cont', {cat:'Konfektion',val:this.konfSel, Spezifikation:this.secondSel,Platzierung:this.thirdSel})
                }
                else{
                    this.$emit('cont', {cat:'Konfektion',val:this.konfSel, Platzierung:this.thirdSel});
                }
            }
        },
        created(){
            this.obenunten = "Oben u. unten (an den "+this.questDict.Dimensions.value.width+"mm langen Seiten)";
            this.linksrechts = "Links u. rechts (an den "+this.questDict.Dimensions.value.height+"mm langen Seiten)";
        }
    }

</script>
