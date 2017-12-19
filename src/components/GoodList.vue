<template>
  <div id="goodList">
    <ul class="goods">
      <Row>
        <Col span = 4>
        <p>商品图片</p>
        </Col>
        <Col span = 5>
        <p>商品名称</p>
        </Col>
        <Col span = 3>
        <p>商品价格</p>
        </Col>
        <Col span = 3>
        <p>商品销量</p>
        </Col>
        <Col span = 3>
        <p>商品评分</p>
        </Col>
      </Row>
      <li v-for="good, index in goods" class="good">
        <span
        @click = "send_Item(good)"
        >
          <li class = "good_inf">
            <div class = "good_image">
              <img :src="good.picture">
            </div>
            <div class = "good_name">
              {{ good.name }}
            </div>
           </li>
           <li class = "good_price">
              <p class="price">￥{{good.price}}</p>
           </li>
           <li class = "good_s_volume">
             <p class = "volume">{{ good.sales_volume }}</p>
           </li>
           <li class = "good_score">
             <p class = "score">{{ good.scores.good }}</p>
           </li>
          </span>
      </li>
    </ul>
  </div>
</template>
<script>
import book from '../assets/data.json'

export default {
  name: 'GoodList',
  props: ['searchArr'],
  mounted(){
    this.sort_arr();
  },
  data: () => ({
    bb: book,
    // use array to save goods, here i use some examples to show the web
    goods: []
  }),
  watch:{
    searchArr: function(new_Arr) {
      this.sort_arr(new_Arr);
    }
  },
  methods: {
    send_Item: function(good) {
      this.$emit ('sendItem', good);
    },
    sort_arr: function(){
      this.goods.splice(this.searchArr.length);
      var i = 0;
      var j = 0;
      var save_arr = [];
      while (i < this.searchArr.length){
        j = 0;
        while (j < this.bb.Books.length){
          if (this.searchArr[i] === this.bb.Books[j].id){
            save_arr.push(this.bb.Books[j]);
            break;
          }
          j ++;
        }
        i ++;
      }
      for (i = 0; i < save_arr.length-1; i ++){
        for(j = i + 1; j < save_arr.length; j ++){
          if (save_arr[j].price < save_arr[i].price){
            var temp = save_arr[j];
            save_arr[j] = save_arr[i];
            save_arr[i] = temp;
          }
          else if(save_arr[j].price === save_arr[i].price){
            if (save_arr[j].sale_volume > save_arr[i].sale_volume){
              var temp = save_arr[i];
              save_arr[i] = save_arr[j];
              save_arr[j] = temp; 
            }
          }
        }
      }
      var k = 0;
      while (k < save_arr.length){
        this.goods.splice(k, 1 ,save_arr[k]);
        k ++;
      }
      this.$emit ('sendItem', this.goods[0]);
      return true;
    }
  } 
}
</script>
<style scoped>
#goodList {
  margin: 0 auto;
}

.goods li {
  list-style: none;
}

.good {
  width: 100%;
  height: 130px;
  border-bottom: 1px solid #e7e7e7;
}

.good li{
  float: left;
  height: 100%;
}
.good .good_inf{
    width: 190px;
}
.good .good_inf .good_image{
    width: 80px;
    height: 80px;
    margin-top: 20px;
    float: left;
}

.good .good_inf .good_image img{
  width: 100%;
  vertical-align: top;
}

.good .good_inf .good_name{
  margin: 20px 0 0 10px;
  line-height: 18px;
  width: 100px;
  float: left;
}

.good .good_price{
  width: 65px;
}

.good .good_price price{
  margin-top: 20px;
  line-height: 18px;
  font-family: Verdana,Tahoma,arial;
  color: #3c3c3c;
  font-weight: bold;
}

.good .good_s_volume{
  width: 60px;

}
.good .good_s_volume volume{
  line-height: 18px;
  margin-top: 20px;
  font-family: Verdana,Tahoma,arial;
  color: #900;
  font-weight: bold;
}

.good .good_score{
  width: 60px;
}

.good .good_score .score{
  line-height: 18px;
  margin-top: 20px;
  font-family: Verdana,Tahoma,arial;
  color: #900;
  font-weight: bold;
}
</style>