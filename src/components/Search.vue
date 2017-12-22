<template>
  <div id="search">
    <div class="nav-search">
      <Input size="large" v-model="my_input" placeholder="请输入关键字" style="width: 500px" @keyup.13="showNextpage"></Input>
      <Button type="primary" icon="ios-search" size="large" @click="showNextpage" style="width: 120px">搜索</Button>
    </div>
    <!--<input type="text" v-model="my_input" id="input_type">-->
    <!--<button v-on:click="showNextpage" id="button_type">搜索</button>-->
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
      my_input:"",
      right_book : books
    }
  },
  methods: {
     showNextpage: function(){
       if(this.my_input === "") {
           this.$Message.warning('搜索内容不能为空！');
           return;
       }
       this.search_result=[];
       this.input_result=this.my_input;
       for(let data of this.right_book.Books){
           let book_name=data.name;
           book_name=book_name.trim().toLowerCase();
           let input_name=this.input_result;
           input_name=input_name.trim().toLowerCase();

           if (book_name.indexOf(input_name) >= 0) {
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

  .nav-search {
    padding-top: 20px;
    padding-bottom: 20px;
  }

</style>
