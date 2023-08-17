<template>
  <img alt="Typico logo" src="./assets/typico.png">
  <h1>Print Order</h1>
  <component :is="currentComponent" @hereyago="Answers"></component>
  <p>{{ possibleMaterialsList }}</p>
</template>

<script>
//import { BaseTransition } from 'vue'
import jsonData from '../newupdated.json'
export default {
  name: 'App',
  data() {
    return {
      resultat: '',
      trial: false,
      currentComponent: null,
      currentComponentI: 0,
      questDict: {
        Dimensions : {
          component: this.renderDimensions,
          value: null,
          
        },
        
        InOut: {
          component: this.renderInOut,
          value: null,
          searchParams: null,
        },
        Konf: {
          component: this.renderKonf,
          value: null,
          searchParams: null,
          description: null,
        },
        doubleSlit: {
          component: this.renderdoubleSlit,
          value: null,
          searchParams: null,
        },
        blickDicht: {
          component: this.renderblickDicht,
          value: null,
          searchParams: null,
        },
        backLit: {
          component: this.renderbackLit,
          value: null,
          searchParams: null,
        },
      
        
        
        
      },
      possibleMaterialsList: [],
      questions: [],
      
    }
    
  },
  methods: {
    renderDimensions: () => import('./components/questions/DimensionInput.vue'),
    renderInOut: () => import('./components/questions/InOut.vue'),
    renderdoubleSlit: () => import('./components/questions/doubleSlit.vue'),
    renderblickDicht: () => import('./components/questions/blickDicht.vue'),
    renderbackLit: () => import('./components/questions/backLit.vue'),
    renderKonf: () => import('./components/questions/Konf/KonfInput.vue'),
    whichFrame: () => import('./components/questions/Konf/FrameExists.vue'),
    renderFileUpload: () => import('./components/pages/FileUpload.vue'),

    loadFirstComponent(){
      this.whichFrame().then(module => {
        this.currentComponent = module.default;
      })
    },

    populateQuestions(){
      Object.keys(this.questDict).forEach((key) =>{
        this.questions.push(key)
      });
    },


    /* loadPossibleMaterials(){
      let newList = []
      if(this.questDict.InOut.value != null){
        newList = this.possibleMaterials("Einsatz", this.questDict.InOut.value);
      }
      if(this.questDict.doubleSlit.value != null){
        if(this.questDict.doubleSlit.value){
          newList = this.updateMaterials(newList, "Einsatz", "beidseitig bedruckbar");
        }
      }
      if(this.questDict.blickDicht.value != null){
        newList = this.updateMaterials(newList, "Einsatz", this.questDict.blickDicht.value);
      }
      if(this.questDict.backLit.value != null){
        if(this.questDict.backLit.value){
          newList = this.updateMaterials(newList, "Einsatz", "hinterleuchtbar");
        }
      }
      if(this.questDict.Konf.value != null){
        if(this.questDict.Konf.value != '')
        {
          newList = this.updateMaterials(newList, "Konfektion", this.questDict.Konf.value)
        }
        
      }

      this.possibleMaterialsList = newList
    }, */

    newUpdateMaterials(currentSelection, searchParams){
      if(searchParams.val == ''){
        return currentSelection;
      }
      let singleChoiceSelection = [];
      if(Array.isArray(searchParams.val)){
        searchParams.val.forEach((value) => {
          const fractionSelection = this.possibleMaterials(searchParams.cat, value)
          fractionSelection.forEach((result) => {
            if(!singleChoiceSelection.includes(result)){
              singleChoiceSelection.push(result);
            }
          })
        })
      }
      else{
        singleChoiceSelection = this.possibleMaterials(searchParams.cat, searchParams.val)
      }

      if(searchParams.disect){
        return currentSelection.filter(item => !singleChoiceSelection.includes(item));
      }
      else if(searchParams.first){
        return singleChoiceSelection;
      }
      else{
        return currentSelection.filter(item => singleChoiceSelection.includes(item));
      }
    },

    /* updateMaterials(currentSelection, cat, val){
      let singleChoiceSelection = []
      
      if(Array.isArray(val)){
        val.forEach((value) => {
          const fractionSelection = this.possibleMaterials(cat, value)
          fractionSelection.forEach((result) => {
            if(!singleChoiceSelection.includes(result)){
              singleChoiceSelection.push(result);
            }
          })
        })
      }
      else{
        singleChoiceSelection = this.possibleMaterials(cat, val)
      }

      


    }, */

    possibleMaterials(category,  value){
      const matCat = Object.keys(jsonData.Material);
      let pMats = []

      for (const cat in matCat){
        const eachmaterial = jsonData.Material[matCat[cat]]
        for (let material in eachmaterial){
          if (!Array.isArray(jsonData.Material[matCat[cat]][material][category]) ){ 
            if(jsonData.Material[matCat[cat]][material][category] == value){
              if(!(material in pMats)){
                pMats.push(material)
              }
            }
          }
          else
          {
            const listshit = jsonData.Material[matCat[cat]][material][category]
            this.trial = listshit
            for(let object in listshit)
            {
              const rightval = listshit[object]
              if(rightval == value){
                if(!(material in pMats)){
                  pMats.push(material)
                }
              }
                
            }
          }
        }
      }
      return pMats
    },

    loadNext() {
      this.currentComponent = null;
      //this.loadPossibleMaterials();
      this.currentComponentI = 0;
      while (this.currentComponentI < this.questions.length && this.questDict[this.questions[this.currentComponentI]].value !== null) {
        this.currentComponentI++;
      }
      if(this.currentComponentI >= this.questions.length){
        this.renderFileUpload().then((module) => {
          this.currentComponent = module.default;
        })
        
      }
      else{
        const func = this.questDict[this.questions[this.currentComponentI]].component
        func().then(module => {
          this.currentComponent = module.default;
      })
      }
      

      
      //this.loadPossibleMaterials()
    },


    doTest(){
      const func = this.renderFlames
      func().then(module => {
        this.currentComponent = module.default;
      })
    },

    Answers(data){
      this.trial = this.currentComponentI
      this.questDict[this.questions[this.currentComponentI]].value = data;
      this.possibleMaterialsList = this.newUpdateMaterials(this.possibleMaterialsList, data)
      this.loadNext()
    },
  },
  mounted() {
    this.populateQuestions()
    this.loadNext()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>