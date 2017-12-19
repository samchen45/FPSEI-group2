<template>
  <div id="search">
    <input type="text" v-model="my_input" id="input_type">
    <button v-on:click="showNextpage" id="button_type">搜索</button>
  </div>
</template>

<script>
import books from '../assets/data.json'
export default {
  name: 'Search',
  data: function(){
    return{
      search_result:[],
      input_result:null,
      my_input:"请输入关键字",
      right_book : books
    }
  },
  methods: {
     showNextpage: function(){
       this.search_result=[];
       this.input_result=this.my_input;
       for(let data of this.right_book.Books){
         if(data.name.indexOf(this.input_result) >= 0){
           this.search_result.push(data.id);
         }
       }
       this.$emit("sendResult",this.search_result);
     }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
#input_type{
  width: 400px;
  height: 30px;
  font-size:20px;
}
#button_type{
  width: 70px;
  height:35px;
  font-size:20px;
}
</style>
