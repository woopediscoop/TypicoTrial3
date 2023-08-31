<template>
    <component :is="currentKonf" @cont="Continue" @final="EmitKonf"></component>
</template>

<script>

/*import jsonData from "C:\\Users\\CLEVO Computer\\Desktop\\JS Frameworks\\Vue\\Tut\\hello-world"*/


export default {

    props: ['lastPage'],
    data(){
        return{
        trial: "",
        ans: null,
        currentKonf: null,
        questionsDict : {
            'frameExists' : null,

        },
        }
    },
    methods: {
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

            const templateStr = data.frameStatus;
            this.$emit('continue', data.lastPage)
            if(templateStr == "frameExists"){
                this.frameExists().then(module => {
                    this.currentKonf = module.default;
                })
                
            } else if (templateStr == "noFrame"){
                this.noFrame().then(module => {
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

