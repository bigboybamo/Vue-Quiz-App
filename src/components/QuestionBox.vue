<template>
<div class="question-box-container">
  <b-jumbotron >
    <template v-slot:lead>
     {{ currentQuestion.question}}
    </template>

    <hr class="my-4"> 
        <b-list-group>
        <b-list-group-item
         v-for="(answer, index) in shuffledAnswers" :key="index"
        @click="selectAnswer(index)"
       :class="[selectedIndex === index ? 'selected' :'']"
       >
            
        {{answer}}</b-list-group-item>
        
        </b-list-group>

    <p>
 
    </p> 

    <b-button 
    variant="primary"
    @click="submitAnswer"
    :disabled="selectedIndex === null"
    > 
        Submit
    </b-button>
    <b-button @click="next" variant="success" href="#">Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

export default {
    props:{
        currentQuestion : Object,
        next: Function,
        increment: Function
    },
    data(){
return {
    selectedIndex:null,
    correctIndex: null,
    shuffledAnswers :[]
}
    },
    methods:{ 
selectAnswer (index){
    this.selectedIndex = index
   
},
submitAnswer(){
let isCorrrect = false
if(this.selectedIndex === this.correctIndex){
    isCorrrect=true
}
this.increment(isCorrrect)
},
shuffleAnswers(){
let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
          
this.shuffledAnswers = _.shuffle(answers)
this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
},

    },
    computed:{
      answers(){
          let answers = [...this.currentQuestion.incorrect_answers]
          
          answers.push(this.currentQuestion.correct_answer)

          return answers
      }
    },

    watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    }
  },
    mounted(){
        console.log(this.currentQuestion)
    }
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;
}

.list-group-item:hover{
   background: #eee;
   cursor: pointer;
}

.btn{

    margin:0 5px;
}

.selected{
    background-color: blue;
}

.correct{
    background-color:green;
}

.incorrect {
    background-color:red;
}
</style>