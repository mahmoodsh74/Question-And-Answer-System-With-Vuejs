<template>
    <div>
        <b-jumbotron>
            <template >{{ cQuestion.question }}</template>

   

                <hr class="my-4">

                    <b-list-group>
                        <b-list-group-item 
                             v-for="(answer,i) in shuffledAnswers" 
                            :key="i"
                            @click="ChangeIndex(i)"
                            :class="correctFunc(i)"
                            >
                             {{ answer }}
                         </b-list-group-item>
                    </b-list-group>


    <b-button 
    variant="primary" 
    @click="submitAnswer"
    :disabled="cindex === null || answered"
    href="#">Submit</b-button>
    <b-button variant="success" href="#" @click="next">Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
 import _ from 'lodash';
export default {
     props: {
          cQuestion : Object,
          next: Function,
          increment: Function,
     },
     data(){
         return{
             cindex : null,
             shuffledAnswers : [],
             correctAnswer : null,
             answered: false
         }
     },
     
     computed: {
         answers(){
             let answers = [...this.cQuestion.incorrect_answers];
             answers.push(this.cQuestion.correct_answer);
             return answers;
         }
     },
     watch:{
         cQuestion : {
             immediate: true,
             handler() {
                 this.cindex = null;
                 this.answered = null;
                 this.correctAnswer = null;
                 this.shuffleAnswers();
             }
         }
     },
     methods:{
        ChangeIndex(i){
            this.cindex = i;
        },
        shuffleAnswers(){
            let answers = [...this.cQuestion.incorrect_answers, this.cQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctAnswer = this.shuffledAnswers.indexOf(this.cQuestion.correct_answer);
        },
        submitAnswer(){
            let is_correct = false;
            if(this.cindex === this.correctAnswer){
                is_correct = true;
            }
            this.answered = true;
            this.increment(is_correct);
        },
        correctFunc(index){
            let answerClass = '';
            if(!this.answered && this.cindex === index){
                answerClass = 'selected';
            }else if(this.answered && index === this.correctAnswer){
                answerClass = 'correct';
            }else if(this.answered && index === this.cindex && index !== this.correctAnswer){
                 answerClass = 'incorrect';
            }
            return answerClass;
        }
     },
}
</script>

<style scoped>
    .list-group-item:hover{
        background-color: #eee;
        cursor: pointer;
    }
    .btn{
        margin: 10px 2px;
    }
    .selected{
        background-color: lightblue;
    }
    .correct{
        background-color: lightgreen;
    }
     .incorrect{
        background-color: red;
    }
</style>