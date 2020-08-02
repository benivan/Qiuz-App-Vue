<template>
  <div id="app">
    <Header
    :noCorrect ="noCorrect"
    :noTotal ="noTotal"
    /> 
    <b-container class="bv-example-row">
  <b-row>
    <b-col sm="6" offset="3">
      <QuestionBox
      v-if="questions.length"
      :currentQuestion ="questions[index]"
      :next ="next"
      :increment ="increment"
      />
    </b-col>  
    
  </b-row>
</b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return { questions: [],
    index: 0,
    noCorrect:0,
    noTotal:0
    }

    
  },
  methods:{
    next(){
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.noCorrect++
      }
      this.noTotal++


    }
  },
  
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=18&difficulty=hard&type=multiple',{
      method: 'get' 
    }).then((response) => {
     return response.json()
    }).then((jsonData) => {
      // console.log(jsonData)
      this.questions = jsonData.results
    })
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
