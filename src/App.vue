<template>
  <img alt="Typico logo" src="./assets/typico.png">
  <h1>Print Order</h1>
  <component :is="currentComponent" :lastPage="lastPage" :qDict="questDict" :possibleMats="possibleMaterialsList" @continue="LastPage" @hereyago="Answers" @selection="OnSelected"></component>
  <component :is="afterSelection" :msg="badSel" :material="selectedMaterial" :qDict="questDict" :qs="questions" @uploaded="ShowInfo"></component>
  <component :is="infoPage" :qDict="questDict"  :qs="questions" @why="ShowInfo"></component>
  <br>
  <br>
  <button v-if="proceeded" @click="Back">Zurück</button>
  <!-- <p v-if="proceeded">Mögliche Materialien nach Selektion: <br>{{ possibleMaterialsList }}</p> -->
</template>

<script>
//import { BaseTransition } from 'vue'
import jsonData from '../newupdated.json'
export default {
  name: 'App',
  data() {
    return {
      badSel: '',
      selectedMaterial: '',
      lastPage:'',
      proceeded: false,
      resultat: '',
      trial: false,
      currentComponent: null,
      currentComponentI: 0,
      afterSelection: '',
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
        /* doubleSlit: {
          component: this.renderdoubleSlit,
          value: null,
          searchParams: null,
        }, */
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
    renderInOut: () => import('./components/questions/doorProps.vue'),
    renderdoubleSlit: () => import('./components/questions/doubleSlit.vue'),
    renderblickDicht: () => import('./components/questions/blickDicht.vue'),
    renderbackLit: () => import('./components/questions/backLit.vue'),
    renderKonf: () => import('./components/questions/Konf/KonfInput.vue'),
    whichFrame: () => import('./components/questions/Konf/FrameExists.vue'),
    renderFileUpload: () => import('./components/pages/FileUpload.vue'),
    renderSelector: () => import('./components/pages/MatSelector.vue'),
    renderContact: () => import('./components/pages/ContactForm.vue'),
    renderInfoPage: () => import('./components/pages/InfoPage.vue'),
    //renderDimensionCheck: ()


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
    reloadMaterials(){
      let currentComp = 1;
      this.possibleMaterialsList = [];
      while(currentComp < this.currentComponentI){
        if(this.questDict[this.questions[currentComp]].value != null){
          this.possibleMaterialsList = this.newUpdateMaterials(this.possibleMaterialsList, this.questDict[this.questions[currentComp]].value);
          
        }
        currentComp++;
      }
    },
    LastPage(data){
      this.lastPage = data.lastPage;
    },

    possibleMaterials(category, value){
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
      this.lastPage = ''
      this.currentComponent = null;
      //this.loadPossibleMaterials();
      this.currentComponentI = 0;
      while (this.currentComponentI < this.questions.length && this.questDict[this.questions[this.currentComponentI]].value !== null) {
        this.currentComponentI++;
      }
      if(this.currentComponentI > 0){
        this.proceeded = true;
      }
      if(this.currentComponentI >= this.questions.length){
        this.renderSelector().then((module) => {
          this.currentComponent = module.default;
        })
        
      }
      else{
        const func = this.questDict[this.questions[this.currentComponentI]].component
        func().then(module => {
          this.currentComponent = module.default;
      })
      }
    },


    doTest(){
      const func = this.renderFlames
      func().then(module => {
        this.currentComponent = module.default;
      })
    },

    Back(){
      this.afterSelection = null;
      if(this.lastPage != '')
      {
        const func = this.questDict[this.questions[this.currentComponentI]].component
        this.currentComponent = '';
        func().then(module => {
          this.currentComponent = module.default;
        })
      }
      else {
        
        this.currentComponentI -= 1;
        
        this.questDict[this.questions[this.currentComponentI]].value = null;
        this.reloadMaterials()
        const func = this.questDict[this.questions[this.currentComponentI]].component
          func().then(module => {
            this.currentComponent = module.default;
        })
        if(this.currentComponentI == 0){
          this.proceeded = false;
        }

      }
    },

    Answers(data){
      //Checking if any of the selections are outside the scope of the system
      if(this.currentComponentI == 0){
        if( ((data.height > 5000) + (data.width > 5000)) == 2 ){
          //Message to ContactForm
          this.badSel = "Für ihre gewollten Dimensionen von " + (data.width) + 'x' + (data.height) + " mm müssen zusätzlich Nähte konfektioniert werden. Lassen Sie sich dafür von unseren Verkäufern beraten."
          this.currentComponent = "";
          this.renderContact().then(module => {
            this.afterSelection = module.default;
          })
          return;
        }
      }
      else if(this.currentComponentI == 3){
        if(data.disect == false){
          //Message to ContactForm 
          this.badSel = "Für eine bestimmte Materialtransparenz wenden Sie sich am besten an unsere Verkäufer."
          this.currentComponent = "";
          this.renderContact().then(module => {
            this.afterSelection = module.default;
          })
          return;
        }
      }
      else if(this.currentComponentI == 4){
        if(data.val == 'hinterleuchtbar'){
          this.badSel = "Für einen hinterleuchtbaren Druck wenden Sie sich am besten an unsere Verkäufer."
          this.currentComponent = "";
          this.renderContact().then(module => {
            this.afterSelection = module.default;
          })
          return;
        } 
      }
        this.questDict[this.questions[this.currentComponentI]].value = data;
        this.possibleMaterialsList = this.newUpdateMaterials(this.possibleMaterialsList, data)
        this.loadNext()
    },

    OnSelected(data){
      this.selectedMaterial = data;
      this.currentComponent = "";
      this.proceeded = false;
      this.renderFileUpload().then(module => {
        this.afterSelection = module.default;
      })

    },

    ShowInfo(){
      this.renderInfoPage().then(module => {
        this.afterSelection = module.default;
      })
    }
  },
  mounted() {
    this.populateQuestions();
    this.loadNext();
    
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