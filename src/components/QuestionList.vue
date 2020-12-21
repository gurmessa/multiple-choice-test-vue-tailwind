<template>
  <div class="max-w-sm mx-auto px-10 sm:px-4">
    <div  v-if="isLoading" class="flex h-screen p-6  max-w-sm mx-auto ">
    <img src="@/assets/img/loading.gif" class=" mx-auto my-auto align-middle "  />
    </div>
  <div v-else>
    <div  class="mb-8" >
      <Question @answerChange="answerChange" v-for="(question,index) in questions" :key="index" :question="question" :index="index" :isResult="isResult" :validate="validate">

      </Question>
    </div>  
  
    <!--<h4 class="my-4" v-if="validate">You must answer all questions before proceeding.</h4> -->
    <button v-if="!isResult" v-on:click="submit" class="px-10 bg-gray-800 hover:bg-gray-700 text-gray-200 px-4 py-2 rounded-md" >Save</button>

     <div v-if="isResult" class="mx-auto font-bold text-gray-700 rounded-full bg-green-300 flex items-center justify-center font-mono w-64 h-64 text-5xl border-4 border-green-400" >Total<br/>{{totalCorrectAnswer}} / {{questionsResult.length}}</div>



   </div>
   </div>
</template>

<script>
import Question from './Question.vue'
import axios from 'axios';
export default {
  name: 'QuestionList',
  components: {
    Question
  },
  data () {
    return {
      validate:false,
      data: null,
      isLoading:true,
      questions:[
         /*{
           text:"What is your name",
           choices:["Amer","choice 2"],
           correctAnswer:"Amer",
           index:2
         },
         {
           text:"What colour will you paint the children's bedroom?",
           choices:["It wasn't very difficult.","We can't decide.",
           "I hope it was right."],
           correctAnswer:"We can't decide.",
           userchoice:"",
           index:1
         }*/

      ],
      questionsResult:[],
      isResult:false,
      totalCorrectAnswer:0,
      /*category:"",
      level:"",*/
      totalQuestions:5
    }

    
  },
  methods: {
    submit() {
      if(this.questions.length > 0 && this.questions.length == this.questionsResult.length){
        this.isResult = true;
        this.displayResult();
      }else{
        this.validate = true;
      }
      //this.isResult = true
      /*this.isValid = this.validate();
      if(this.isValid){
        this.checkAnswer()
      }*/
    },
    /*validate: function(){
      var i=0
      while (i<this.questions.length  ) {
        if(this.questions[i]["userchoice"] === ""){
          return false;
        }
        i++;
      }

      return true;
    },*/
    /* Randomize array in-place using Durstenfeld shuffle algorithm */
    shuffleArray(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }

        return array;
    },
    answerChange(data) {
      this.questionsResult[data['id']] = data['isCorrect'];
    },
    displayResult(){
        console.log("displayResult");
        console.log(this.questionsResult);
        this.questionsResult.forEach((value)=> {
          if (value) {
              this.totalCorrectAnswer +=1;
          }
          window.scrollTo(0,document.body.scrollHeight);
        })  
    }

  },
  computed:{
    completed(){
      return this.userchoice!=""
    }
  },
  props: [
      'level',
      'categoryId'
    ]
  ,
  mounted () {
    var info ;
    axios
      .get('https://opentdb.com/api.php?amount='+this.totalQuestions+'&category='+this.categoryId+'&difficulty='+this.level)
      //.then(response => (info = response.data.results))
       .then( (response) => {
          this.data = response.data.results;
          info = response.data.results;
          info.forEach((result,index)=>{
            var choices = result['incorrect_answers']
            choices.push(result['correct_answer'])
            //console.log(choices)
            let randomChoices= this.shuffleArray(choices)
            //console.log(randomChoices);
            let obj= {
               "text": result['question'],
               "choices":randomChoices,
               "correctAnswer":result['correct_answer'],
               "id":index,
               "userchoice":""
            }
            this.questions.push(obj);
          })

          this.isLoading= false;
        })
        .catch( (error)=> {
          console.log(error);
        })
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
