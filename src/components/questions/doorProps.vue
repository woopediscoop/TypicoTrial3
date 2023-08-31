<template>
    <component :is="currentComponent" @continue="renderTheWeather" @final="EmitIt"></component>
</template>

<script>

export default{
    data(){
        return  {
            currentComponent: ""
        }
    },
    methods:{
        renderInOut: () => import('../questions/InOut.vue'),
        renderWeather: () => import('../questions/wetherItWeathers.vue'),
        renderTheWeather(){
            this.renderWeather().then(module => {
                this.currentComponent = module.default;
            })
        },
        EmitIt(data){
            this.$emit('hereyago', data);
        }
    },
    created(){
        this.renderInOut().then(module => {
            this.currentComponent = module.default;
        })
    }
}



</script>
