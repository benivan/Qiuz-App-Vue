<template>
  <div class="question-box-container">
    <b-jumbotron>
      <div>
        <b-jumbotron>

          <template v-slot:lead>
            {{currentQuestion.question}}
          </template>

          <hr class="my-4" />

          <b-list-group>
            <b-list-group-item
             v-for="(answer,index) in answers" 
             :key="index"
             @click="selectAnswer(index)"
             :class="answerClass(index)"
            >
                {{answer}}
              
              </b-list-group-item>
            
          </b-list-group>

          <b-button 
          @click="submitAnswer" variant="primary" 
          :disabled="selectedindex === null || answered"
          >
            Submit
            </b-button> 

          <b-button
           @click="next" variant="success" >
            Next
            </b-button>
        </b-jumbotron>
      </div>
    </b-jumbotron>
  </div>
</template>

<script>
import  _ from 'lodash'
export default {
  props:{
    currentQuestion:Object,
    next: Function,
    increment:Function
  },

  computed:{
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
     return answers
    }

  },

  data(){
    return{
      selectedindex:null,
      correctindex:null,
      answered: false,
      shuffledAnswers:[]
    }
  },

  watch:{ 
    currentQuestion:{
      immediate:true,
      handler(){
        this.selectedindex= null
        this.shufflelAnswers()
        this.answered = false
      }
    }
  },
  methods:{
    selectAnswer(index){
      this.selectedindex = index
      
    },
    submitAnswer(){
      let isCorrect =false
      if(this.selectedindex === this.correctindex){
        isCorrect= true
      }
      this.answered = true
      this.increment(isCorrect)
    },
    shufflelAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctindex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){

      if( !this.answered && this.selectedindex === index){
        return 'selected'
      }
      else if (this.answered && this.correctindex === index) {
        return 'correct'
        
      } else if (this.answered && 
        this.selectedindex ===index && 
        this.correctindex !== index
        ){
        return 'incorrect'
        
      }
    
    }
  }
  
}
</script>
<style scoped>
.list-group{
  margin-bottom: 15px;

}
.list-group-item{
  background-color: #EEE;
  transition-duration: 0.5s;
}
.list-group-item:hover{
  background-color:lightsteelblue;
  cursor: pointer;

}
.btn{
  margin: 0 5px;
}
.selected{
  background-color: lightslategray;
}
.correct{
  background-color: lightgreen;
}
.incorrect{
  background-color:lightcoral;
}
</style>