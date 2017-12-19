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
       this.input_result=this.my_input;
       let i=0;
       for(let data in this.right_book.Books){
         if(this.right_book.Books[i].name===this.input_result){
           this.search_result.push(this.right_book.Books[i].id);
         }
         i++;
       }
       this.$emit("sendBookId",this.search_result);
     }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
#input_type{
  position:absolute;
  top:10px;
  left:250px;
  width: 400px;
  height: 30px;
  font-size:20px;
}
#button_type{
  position:absolute;
  top:10px;
  left:654px;
  width: 70px;
  height:35px;
  font-size:20px;
}
</style>
