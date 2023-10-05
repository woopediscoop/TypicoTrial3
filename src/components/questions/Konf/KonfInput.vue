<template>
    <component :is="currentKonf" @cont="Continue" @final="EmitKonf" :questDict="this.qDict" :konfList="konfList" :visData="visData"></component>
</template>

<script>

/*import jsonData from "C:\\Users\\CLEVO Computer\\Desktop\\JS Frameworks\\Vue\\Tut\\hello-world"*/


export default {

    props: ['lastPage', 'qDict'],
    data(){
        return{
            konfList: [],
            visData:null,
            ans: null,
            currentKonf: null,
            questionsDict : {
                'frameExists' : null,

            },
        }
    },
    methods: {
        visKonf: () => import('./VisKonf.vue'),
        dimensionCheck: () => import('./DimensionCheck.vue'),
        frameCondition: () => import('./FrameExists.vue'),
        frameExists: () => import('./ifFrameExists.vue'),
        noFrame: () => import('./noFrame.vue'),
        frameSale: () => import('./orderFrame.vue'),
        allRoundFrame: () => import('./AllRoundFrame.vue'),
        contactForm: () => import('../../pages/ContactForm.vue'),
        firstQuestion() {
            this.frameCondition().then(module => {
                this.currentKonf = module.default;
            })
        },
        Continue(data){
            this.visData = data;
            if(data.val != null){
                if(data.val == 'Hohlsaum' || data.val == 'Kederschlaufe'){
                    this.dimensionCheck().then(module => {
                        this.currentKonf = module.default;
                    })
                }
                else{
                    this.EmitKonf(data)
                }
            }
            else{
                //Outdated
            const templateStr = data.frameStatus;
            this.$emit('continue', data.lastPage)
            if(templateStr == "frameExists"){
                this.konfList = [
                    {label:'Gummilippe',value:'Gummilippe'},
                    {label:'Kederschlaufe',value:'Kederschlaufe'},
                    {label:'Ösen',value:'Ösen'}
                ] //Maybe add Hohlsaum
                this.visKonf().then(module => {
                    this.currentKonf = module.default;
                })
                
            } else if (templateStr == "noFrame"){
                this.konfList = [
                    {label:'Flausch u. Klett',value:'Klett'},
                    {label:'Hohlsaum',value:'Hohlsaum'},
                    {label:'Ohne Konfektion',value:''}
                ]
                //this.konfList = ['Flausch u. Klett', 'Hohlsaum', 'Ohne Konfektion']
                this.visKonf().then(module => {
                    this.currentKonf = module.default;
                })
            } else if (templateStr == "frameSale"){
                this.frameSale().then(module => {
                    this.currentKonf = module.default;
                })
            } else if (templateStr == "allRoundFrame"){
                
                this.allRoundFrame().then(module => {
                    this.currentKonf = module.default;
                })
            } else if (templateStr == "contactForm"){
                this.contactForm().then(module => {
                    this.currentKonf = module.default;
                })
            }
            }
            










            

        },
        EmitKonf(konfData) {
            this.trial = konfData;
            this.$emit('hereyago', konfData);
        }

    },
    created(){
        if(this.lastPage == "frameExists"){
            this.frameExists().then(module => {
                    this.currentKonf = module.default;
                })
        }
        else{
            this.firstQuestion()
        }
    }
}
</script>

