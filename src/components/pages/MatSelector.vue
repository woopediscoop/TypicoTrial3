<template>
    <h4>Auswahl:</h4>
    <p>{{ ratetrial }}</p>
    <button v-if="proceed" @click="Selection">Weiter</button>
</template>

<script>
import jsonData from '../../../newupdated.json'

    export default{
        props: ['possibleMats', 'qDict'],
        data(){
            return{
                materials: this.possibleMats,
                ratetrial: "",
                proceed: false,
                finalMaterial: "",
            }
        },
        methods:{
            Selection(){
                this.$emit('selection', this.finalMaterial)
            },
            FirstBest(matList, maxRating){
                const all = this.ChooseBest(matList, maxRating);
                if(all != null){
                    const props = this.GetProperties(all[0]);
                    return props;
                }
                else {
                    return null;
                }
               
            },
            ChooseBest(matList, maxRating){
                let curation = [];
                Object.getOwnPropertyNames(jsonData.Material).forEach(cat => {
                    Object.getOwnPropertyNames(jsonData.Material[cat]).forEach(key => {
                        for(const i in matList){
                            if(key == matList[i]){
                                if(this.FindRating(key) == maxRating){
                                    curation.push(key)
                                }
                            }
                        }
                    })
                })
                return curation
            },
            BestRating(matList){
                if (matList.length == 0){
                    return "nixdiese";
                }
                else{
                    let ratings = []
                    matList.forEach(mat => {
                        ratings.push(this.FindRating(mat))
                    })
                    const topRating = Math.max(...ratings)
                    return topRating;
                }
            },
            GetProperties(mat){
                let props = {};
                Object.getOwnPropertyNames(jsonData.Material).forEach(cat => {
                    Object.getOwnPropertyNames(jsonData.Material[cat]).forEach(key => {
                        if(key == mat){
                            props = {
                                "Name" : mat,
                                "BaseText" : jsonData.Material[cat][key]["BaseText"],
                                "Einsatz" : jsonData.Material[cat][key]["Einsatz"],
                                "Grüne Eigenschaften" : jsonData.Material[cat][key]["Grüne Eigenschaften"],
                            } 
                        }
                    })
                })
                return props;
            },
            FindRating(mat){
                let rating = 0;
                Object.getOwnPropertyNames(jsonData.Material).forEach(key => {
                    Object.getOwnPropertyNames(jsonData.Material[key]).forEach(matKey => {
                        if(matKey == mat){
                            if(jsonData.Material[key][mat]["Rating"] == null){
                                rating = 3;
                            }
                            else{
                                rating = jsonData.Material[key][mat]["Rating"];
                            }
                        }
                    })
                })
                return rating
            },
            SalesText(mat){
                const matProps = this.GetProperties(mat);
                let fullStop = false;
                if(matProps.BaseText != null){
                    this.ratetrial += " " + matProps.BaseText;
                    fullStop = true;
                } else
                {

                    //In/Outdoor
                    if(this.qDict.InOut.value.val == "Outdoor" || ['Outdoor','Weathered']){
                        this.ratetrial += " hält den statischen und witterungsbedingten Spannungen, die mit einer Verwendung im Außenbereich kommen fabelhaft stand.";
                        fullStop = true;
                    }
                    else {
                        this.ratetrial += " ist geeignet für den Innenbereich.";
                        fullStop = true;
                    }

                    // Transparenz
                    if(this.qDict.blickDicht.value.val == ['Öffnungsanteil 15 %', 'Öffnungsanteil 8 %', 'Öffnungsanteil 35 %', 'transparent', 'lichtdurchlässig', 'Öffnungsanteil 28 %']){
                        if(this.qDict.blickDicht.value.disect){
                            this.ratetrial += " Es ist undurchsichtig"
                            fullStop = false;
                        }
                        else {
                            this.ratetrial += " Es ist transparent"
                            fullStop = false;
                        }
                    }
                    else if (this.qDict.blickDicht.value.val == ['absolut licht- und blickdicht', 'lichtdicht', 'absolut lichtdicht', 'blickdicht']){
                        this.ratetrial += " Es ist komplett lichtundurchlässig und blickdicht"
                        fullStop = false;
                    }
                    if(fullStop){
                        this.ratetrial += "."
                    }
                }
            }
        },
        created(){
            this.proceed = true;
            const dvv = this.FirstBest(this.possibleMats, this.BestRating(this.possibleMats));
            if(dvv != null &&  dvv.Name != undefined){
                this.finalMaterial = dvv.Name;
                this.Selection();
                this.ratetrial += "Wir empfehlen dasas Material " + dvv.Name +", es";
                this.SalesText(dvv.Name)
            }
            else {
                this.ratetrial = "Es gibt kein passendes Material für Ihre Selektion. Bitte wenden Sie sich an einen Verkäufer."
            }
        }


    }

</script>
