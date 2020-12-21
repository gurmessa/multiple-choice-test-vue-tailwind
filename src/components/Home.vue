<template>
  <div>
    <Levels @onLevelSelect="onLevelSelect" v-if="currentState=='levels'"></Levels>
    <Category @onCategorySelect="onCategorySelect" v-else-if="currentState=='category'"></Category>
    <QuestionList :level="level" :categoryId="categoryId" v-else-if="currentState=='questionList'"></QuestionList>
  </div>

</template>

<script>
import QuestionList from './QuestionList.vue'
import Levels from './Levels.vue'
import Category from './Category.vue'

//import axios from 'axios';
export default {
  name: 'Home',
  components: {
    QuestionList,Category,Levels
  },
  data () {
    return {
      currentState:"levels",
      level:"",
      categoryId:0
    }
  },
  methods: {
    select(){
      console.log("here");
      alert(this.selectedLevel)
    },
    onLevelSelect(selectedLevel){
      this.level = selectedLevel;
      this.currentState = "category";
    },
    onCategorySelect(categoryId){
      this.categoryId = categoryId;
      this.currentState= "questionList"
    }
  },
  computed:{
    
  },
  mounted () {

  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
