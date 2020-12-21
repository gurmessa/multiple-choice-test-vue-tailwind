<template>

  <div class="max-w-sm sm:max-w-2xl   mx-auto">
    <div  v-if="isLoading" class="flex h-screen p-6  max-w-sm mx-auto ">
    <img src="@/assets/img/loading.gif" class=" mx-auto my-auto align-middle "  />
    </div>

    <div  class="mb-8 flex flex-wrap" v-else> <!---questions -->


      <div class=" flex my-auto w-full">
        
        <span class="mx-auto text-left text-lg font-semibold m-1" >Choose Category </span>

      </div>
      <div  v-for="category in categories" :key="category.id" class="w-full sm:w-1/2">
      <input @change="select" v-model="categoryId" :value="category.id" class="hidden checked:bg-gray-900 checked:border-transparent" type="radio" :name="'category'+category.id" :id="'category'+category.id" />
      <label   

 class="mx-2 checked:bg-red-600 flex my-2 text-base rounded-md hover:border-gray-500 border-gray-300 px-6 py-3 border-2 bg-gray-100" :for="'category'+category.id" >{{category.name}}</label>

      </div>
    </div>
</div>


</template>

<script>
//import QuestionList from './QuestionList.vue'
import axios from 'axios';
export default {
  name: 'Category',
  components: {
    //QuestionList
  },
  data () {
    return {
      categories:[],
      categoryId:0,
      isLoading :true
    }
  },
  methods: {
      select(){
        this.$emit('on-category-select' ,this.categoryId);
      } 
  },
  computed:{
    
  },
  mounted () {
    axios
      .get('https://opentdb.com/api_category.php')
       .then( (response) => {
          this.categories = response.data.trivia_categories;
          this.isLoading = false;
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
