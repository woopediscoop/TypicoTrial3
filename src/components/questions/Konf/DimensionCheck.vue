<template>
    <h3>Zugabe</h3>
    <p>Sind Ihre eingegebenen Dimensionen {{ this.questDict.Dimensions.value.width }}x{{ questDict.Dimensions.value.height }} mm mit oder ohne Zugabe {{ konfString }}?</p>
    <button @click="Continue(true)">Zugabe ist inkludiert</button>
    <button @click="Continue(false)">Zugabe ist exkludiert</button>
</template>

<script>

    export default{
        data(){
            return{
                konfData: this.visData,
                konfString: ''
            }
        },
        props: ['visData', 'questDict'],
        methods:{
            Continue(zugabe){
                this.konfData.Zugabe = zugabe;
                this.$emit('final',this.konfData);
            },
        },
        created(){
            if(this.visData.val == 'Kederschlaufe'){
                this.konfString = "der Kederschlaufe";
            }
            else if(this.visData.val == 'Hohlsaum'){
                this.konfString = "des Hohlsaums";
            }
        }
    }

</script>