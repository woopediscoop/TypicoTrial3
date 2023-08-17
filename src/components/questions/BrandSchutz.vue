<template>
    <h3>Brauchen Sie einen speziellen Brandschutz?</h3>
    <label for="konf"></label>
    <select id="konf" name="konf" v-model="selectedKonf" @click="checkSubmitable">
      <option v-for="konf in konfList" :key="konf" :value="konf">{{ konf }}</option>
    </select>
    <button :disabled="disabled" @click="Submit">Weiter</button>
</template>

<script>

import jsonData from '../../../newupdated.json'

    export default {
        data(){
            return {
            selectedKonf: '',
            konfList: Object.keys(jsonData.flammhemmend),
            disabled: true,
            }
        },
        methods: {
            checkSubmitable(){
                if (this.selectedKonf != '')
                {this.disabled = false}
            },
            Submit(){
                if (this.selectedKonf != ''){
                    this.$emit('hereyago', this.selectedKonf)
                }
            },
        },
    }
</script>
