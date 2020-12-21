<template>
  <div class="max-w-sm   mx-auto">
    <div  class="mb-8" > <!---questions -->


      <div class=" flex my-auto">
        
        <span class="mx-auto text-left text-lg font-semibold m-1" >Choose level </span>

      </div>
      <template  v-for="(level,i) in levels" :key="i" class="">
      <input @change="select" v-model="selectedLevel" :value="level" class="hidden checked:bg-gray-900 checked:border-transparent" type="radio" :name="'level'+i" :id="'level'+i" />
      <label   

 class="mx-2 flex my-2 text-base rounded-md hover:border-gray-500 border-gray-300 px-6 py-3 border-2 bg-gray-100" :for="'level'+i" >{{level}}</label>

      </template>
    </div>
</div>

</template>

<script>
//import QuestionList from './QuestionList.vue'
//import axios from 'axios';
export default {
  name: 'Home',
  components: {
    //QuestionList
  },
  data () {
    return {
      levels:["Easy","Medium","Hard"],
      selectedLevel:""
    }
  },
  methods: {
    select(){
      this.$emit('on-level-select' ,this.selectedLevel.toLowerCase());
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
