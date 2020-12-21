<template>
    <!-- result -->
    <div  class="mb-8"  v-if="isResult"> <!---questions -->
      <div class=" flex my-auto">
        <span :class="{'bg-green-600':userchoice== question.correctAnswer ,'bg-red-600':userchoice!= question.correctAnswer }" class="px-2 m-1 rounded-full  text-gray-100 h-6 w-6 my-auto">{{index+1}}</span>
        <span class="text-left text-lg font-semibold m-1" :class="{'text-green-600':userchoice== question.correctAnswer ,'text-red-600':userchoice!= question.correctAnswer }" v-html="question.text"> </span>

      </div>
      <template  v-for="(choice,i) in question.choices" :key="choice" class="">
      <input disabled v-model="userchoice" :value="choice" class="hidden checked:bg-gray-900 checked:border-transparent" type="radio" :name="'question'+index" :id="index+'q'+i" />
      <label   v-bind:class="{ 'bg-green-300': userchoice==choice && choice == question.correctAnswer , 'bg-red-200 line-through text-red-600': userchoice==choice && choice != question.correctAnswer , 'border-green-300 bg-green-200': choice==question.correctAnswer } "

 class="flex justify-between my-2 text-base rounded-md px-6 py-3 border-2 bg-gray-100" :for="index+'q'+i">
        <p v-html="choice">
        </p>
       <svg v-if="userchoice==choice && choice != question.correctAnswer" class="fill-current text-red-500 w-4 h-4 mx-4 my-auto " version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
         viewBox="0 0 208.891 208.891" style="enable-background:new 0 0 208.891 208.891;" xml:space="preserve">
      <path d="M0,170l65.555-65.555L0,38.891L38.891,0l65.555,65.555L170,0l38.891,38.891l-65.555,65.555L208.891,170L170,208.891
        l-65.555-65.555l-65.555,65.555L0,170z"/>
      <g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g><g></g>
      <g></g><g></g><g></g><g></g><g></g><g></g>
      </svg>


<svg  v-if="userchoice==choice && choice == question.correctAnswer" class="fill-current text-green-500 w-4 h-4 mx-4 my-auto " height="417pt" viewBox="0 -46 417.81333 417" width="417pt" xmlns="http://www.w3.org/2000/svg"><path d="m159.988281 318.582031c-3.988281 4.011719-9.429687 6.25-15.082031 6.25s-11.09375-2.238281-15.082031-6.25l-120.449219-120.46875c-12.5-12.5-12.5-32.769531 0-45.246093l15.082031-15.085938c12.503907-12.5 32.75-12.5 45.25 0l75.199219 75.203125 203.199219-203.203125c12.503906-12.5 32.769531-12.5 45.25 0l15.082031 15.085938c12.5 12.5 12.5 32.765624 0 45.246093zm0 0"/></svg>


      </label>


      </template>
    </div>



    <div  class="mb-8" v-else> <!---questions -->


      <div class=" flex my-auto">
        <span class="px-2 m-1 rounded-full bg-black text-gray-100 h-6 w-6 my-auto">{{index+1}}</span>
        <span class="text-left text-lg font-semibold m-1" v-html="question.text"> </span>

      </div>
      <template  v-for="(choice,i) in question.choices" :key="choice" class="">
      <input v-model="userchoice" :value="choice" class="hidden checked:bg-gray-900 checked:border-transparent" type="radio" :name="'question'+index" :id="index+'q'+i" />
      <label   v-bind:class="{ 'bg-gray-400': userchoice==choice  }"
          class="checked:bg-red-600 flex my-2 text-base rounded-md hover:border-gray-500 border-gray-300 px-6 py-3 border-2 bg-gray-100" :for="index+'q'+i" v-html="choice"></label>

      </template>
      <div v-show="validate && !isValid" class="bg-red-200 relative text-red-600 py-1 px-3 rounded-lg">
          You did not provide an answer.
      </div>
    </div>


</template>

<script>
export default {
  name: 'Question',
  data() {
	return {
		userchoice:"",
    isChecked:true,
		/*question:{
			text:"What is your name",
			choices:["Amer","choice 2"],
			correctAnswer:"hello",
			userchoice:"",
			index:1
		}*/
	}
  },
  props: {
    question: { type :Object, required: true },
    index: Number,
    isResult: Boolean,
    validate:Boolean
  },
   watch: {
    userchoice: function () {
      this.$emit('answer-change', {"id":this.question.id,"isCorrect":this.isCorrect})
    }

  },
  computed:{
    isValid: function(){
      return this.userchoice!=""
    },
    isCorrect: function(){
      return this.userchoice === this.question.correctAnswer;
    }
  }

}
</script>

