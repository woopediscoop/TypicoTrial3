<template>
    <h3>Haben Sie einen Rahmen?</h3>
    <button class="sel" @click="FrameStatus('frameExists')">Ein bestehender Rahmen existiert</button>
    <button class="sel" @click="FrameStatus('noFrame')">Der Druck wird an keinem Rahmensystem befestigt</button>
    <button class="sel" @click="FrameStatus('frameSale')">Ich würde gerne einen Rahmen mitbestellen</button>

    
</template>

<script>

    export default{
        data(){
            return{
            }
        },

        methods: {
            FrameStatus(frameStatus){
                this.trial = frameStatus;
                this.$emit('cont', {'frameStatus':frameStatus, 'lastPage':'FrameCondition'})
            },

        }

    }

</script>
