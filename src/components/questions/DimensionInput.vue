

<template>
    <h3>Wie groß ist ihr Druck? Angabe in mm:</h3>
    <p>Höhe in mm:</p>
    <p style="color:red; ">{{ heightmess }}</p>
    <input type="number" v-model="height" @change="heightChange">
    <p>Breite in mm:</p>
    <p style="color:red; ">{{ widthmess }}</p>
    <input type="number" v-model="width" @change="widthChange">
    <p>{{ fehlermeldung }}</p>
    <button :disabled="notSubmitable" @click="submitDimensions">Weiter</button>
</template>

<script>

    export default {
        name: "DimensionInput",
        data(){
            return {
                height: '',
                width: '',
                heightmess: "",
                widthmess: "",
                notSubmitable: true,
            }
        },
        methods: {
            heightChange() {
                if (this.height > 50000){
                    this.heightmess = "Höher als 50 Meter nicht möglich!"
                }
                else{
                    this.heightmess = ""
                }
                this.checkSubmitable()
            },
            widthChange(){
                if (this.width > 50000){
                    this.widthmess = "Breiter als 50 Meter nicht möglich!"
                }
                else{
                    this.widthmess = ""
                }
                this.checkSubmitable()
            },
            checkSubmitable(){
                this.notSubmitable = !(this.height >= 10 && this.width >= 10 && this.height <= 50000 && this.width <= 50000)
            },
            submitDimensions(){
                    const dimensions = { height: this.height, width: this.width}
                    this.$emit('hereyago', dimensions)
                
            }
        }

    }


</script>

