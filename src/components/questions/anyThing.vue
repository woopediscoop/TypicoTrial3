<template>
    <h3>Welche Konfektion?</h3>
    <label for="konf"></label>
    <select id="konf" name="konf" v-model="selectedKonf" @click="checkSubmitable">
      <option v-for="konf in konfList" :key="konf" :value="konf">{{ konf }}</option>
    </select>
    <button :disabled="disabled" @click="Submit">Weiter</button>
</template>

<script>

import jsonData from '../../../updated.json'

    export default {
        
        data(){
            return {
                
                selectedKonf: '',
                wrkrnd: '',
                konfList: Object.keys(jsonData.Konfektion),
                disabled: true,
            }
        },
        methods: {
            checkSubmitable(){
                if (this.selectedKonf != '')
                {this.disabled = false;
                }

            },
            Submit(){
                this.$nextTick(() => {
                    const nib = this.selectedKonf 
                    this.$emit('hereyago', nib)
                });
                
            },
        },
        
        
    }
</script>
