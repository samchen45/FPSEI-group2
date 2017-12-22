<template>
  <div id="goodList">
    <div class="goods">
      <div class="search-result">
        <Row type="flex" align="middle" justify="space-between">
          <Col span = 18>
          <!--<Button type="primary" size="small" @click="showUsedBooks" style="width: 150px;">显示/不显示二手书</Button>-->
          <RadioGroup v-model="selector_option" @on-change="select_data" class="radio-selector">
            <Radio label="0">全部</Radio>
            <Radio label="1">不含二手书</Radio>
            <Radio label="2">仅含二手书</Radio>
          </RadioGroup>
          </Col>
          <Col span=5>
          <span class="record-text">共{{ total_goods }}条记录</span>
          </Col>

        </Row>
      </div>


      <div v-if = "searchArr.length === 0">
        <Row>
          <Col span = 8 offset = 8>
            <Card>
              <p>无搜索结果</p>
            </Card>
          </Col>

        </Row>
      </div>
      <div v-else>
        <div v-for="good, index in showing_goods" class="good">
          <div class="item-padding">
            <div class="good-item" @click = "send_Item(good)">
              <Card>
                <Row>
                  <Col span = 6>
                    <img :src="good.picture" height="70px" width="70px">
                  </Col>
                  <Col span = 5>
                    <p class = "good_name">
                      <img :src="seller_img[good.seller]" alt="" height="20px" width="20px">
                      <!--<Tag v-if = "good.isUsedbooks === 1" class = "Used" color="yellow" type="border">二手</Tag>-->
                      <span class="good-used-tag-padding">
                        <span v-if="good.isUsedbooks === 1" class="good-used-tag">二手</span>
                      </span>
                      {{ good.name }}
                    </p>
                  </Col>
                  <Col span = 7 offset = 1>
                    <p class="price">￥{{good.price.toFixed(2)}}
                        <span v-if="good.freight === 0" class="good-freight-tag">包邮</span>
                    </p>
                    <p v-if="good.freight > 0" class="keys">运费&nbsp;&nbsp;&nbsp;<span class="card-digit">￥{{ good.freight.toFixed(2) }}</span></p>
                    <p class = "keys">月售&nbsp;&nbsp;&nbsp;<span class="card-digit">{{ good.sales_volume }}</span>&nbsp;件</p>
                    <p class = "keys">评分&nbsp;&nbsp;&nbsp;<span class="card-digit">{{ good.scores.good.toFixed(1) }}</span></p>
                  </Col>
                  <Col span = 5>
                    <!--<img :src="seller_img[good.seller]" height="20px" width="58px">-->
                    <div v-for = "tag_id in good.tags" class = "tag">
                      <Tag>{{tags_inf[tag_id]}}</Tag>
                    </div>
                  </Col>
                </Row>
              </Card>
            </div>
          </div>
        </div>
        <div v-if="total_goods > page_goods" class="good-page-switch">
          <Page size="small" :total="total_goods" :page-size="page_goods" @on-change="choose_page"></Page>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import book from '../assets/data.json'

export default {
  name: 'GoodList',
  props: ['searchArr'],
  mounted(){
    this.sort_arr();
    this.total_goods = this.goods.length;
    this.choose_page(1);
  },
  data: () => ({
    status : true,
    bb: book,
    tags_inf: ["正品保证","极速退款","七天退换","有赠品"],
//    seller_img: ["https://img.alicdn.com/tfs/TB1_uT8a5ERMeJjSspiXXbZLFXa-143-59.png",
//      "https://img14.360buyimg.com/da/jfs/t7366/203/1731206510/2597/d71c891f/59a056c1N5e4d6940.png",
//      "https://img.alicdn.com/tfs/TB1MaLKRXXXXXaWXFXXXXXXXXXX-480-260.png",
//      "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=3455760616,3212888784&fm=27&gp=0.jpg",
//      "https://login.dangdang.com/images/logo.png"],
    seller_img: [
      "/src/assets/taobao.png",
      "/src/assets/jingdong.png",
      "/src/assets/tianmao.png",
      "/src/assets/amazon.png",
      "/src/assets/dangdang.png"
    ],

    goods: [],
    total_goods: 0,
    showing_goods: [],
    current_page: 1,
    page_goods: 6,
    selector_option: 0
  }),
  watch:{
    searchArr: function(new_Arr) {
      this.sort_arr(new_Arr);
      this.total_goods = this.goods.length;
      this.current_page = 1;
      this.choose_page(1);
    }
  },
  methods: {
    send_Item: function(good) {
      this.$emit ('sendItem', good);
    },
    sort_arr: function(){
      if (this.searchArr.length === 0){
        this.$emit ('sendItem',[]);
      }
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
            if (save_arr[j].sales_volume > save_arr[i].sales_volume){
              var temp = save_arr[i];
              save_arr[i] = save_arr[j];
              save_arr[j] = temp;
            }
            else if (save_arr[j].sales_volume === save_arr[i].sales_volume){
              if (save_arr[j].scores.good > save_arr[i].scores.good){
                var temp = save_arr[i];
                save_arr[i] = save_arr[j];
                save_arr[j] = temp;
              }
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
    },
    select_data: function (cur) {
      this.sort_arr();
      if (cur === "1") {
        var temp = [];
        for (var i = 0; i < this.goods.length; i ++){
          if (this.goods[i].isUsedbooks === 0){
            temp.push(this.goods[i]);
          }
        }
        var k = 0;
        this.goods.splice(temp.length);
        while (k < temp.length){
          this.goods.splice(k, 1, temp[k]);
          k ++;
        }
        this.$emit ('sendItem', this.goods[0]);
      } else if (cur === "2") {
        var temp = [];
        for (var i = 0; i < this.goods.length; i ++){
          if (this.goods[i].isUsedbooks === 1){
            temp.push(this.goods[i]);
          }
        }
        var k = 0;
        this.goods.splice(temp.length);
        while (k < temp.length){
          this.goods.splice(k, 1, temp[k]);
          k ++;
        }
        this.$emit ('sendItem', this.goods[0]);
      }
      this.total_goods = this.goods.length;
      this.choose_page(1);
    },
    showUsedBooks: function() {
      if (this.status === false) {
        this.sort_arr();
        this.status = true;
        this.total_goods = this.goods.length;
        this.choose_page(1);
      }
      else{
        var temp = [];
        for (var i = 0; i < this.goods.length; i ++){
          if (this.goods[i].isUsedbooks === 0){
            temp.push(this.goods[i]);
          }
        }
        var k = 0;
        this.goods.splice(temp.length);
        while (k < temp.length){
          this.goods.splice(k, 1, temp[k]);
          k ++;
        }
        this.$emit ('sendItem', this.goods[0]);
        this.status = false;
        this.total_goods = this.goods.length;
        this.choose_page(1);
      }
      return true;
    },
    choose_page: function (page) {
      if (this.total_goods <= this.page_goods * page) {
        this.showing_goods = this.goods.slice((page - 1) * this.page_goods, this.total_goods);
      } else {
        this.showing_goods = this.goods.slice((page - 1) * this.page_goods, page * this.page_goods);
      }
    }
  }
}
</script>
<style>

  .item-padding {
    padding: 8px 0;

  }

  .good-item {
    cursor: pointer;
  }

  .good-page-switch {
    padding: 20px 0;
    text-align: center;
  }

  .search-result {
    padding: 10px 0;
  }

  .record-text {
    /*padding-top: 30px;*/
  }

  .code-row-bg{
    padding-top : 10px;
  }

  .Used{
    /*color: #ff0000;*/
  }

  .price{
    color: #ff6600;
    font-weight: bold;
  }

  .radio-selector {
    padding-left: 10px;
  }

  .keys {
      font-size: 10px;
  }

  .card-digit {
      font-size: 12px;
      font-weight: bold;
  }

  .good_name {
      font-weight: bold;
      color: #444
  }

  .good-freight-tag {
      font-weight: normal;
      font-size: 12px;
      color: #006400;
      border: solid 1px;
      border-radius: 3px;
      padding: 1px 2px;
  }

  .good-used-tag {
      font-weight: normal;
      font-size: 12px;
      color: #ff4500;
      border: solid 1px;
      border-radius: 3px;
      padding: 1px 2px;
  }

  .good-used-tag-padding {
  }

/*#goodList {*/
  /*margin: 0 auto;*/
/*}*/

/*.goods li {*/
  /*list-style: none;*/
/*}*/

/*.good {*/
  /*width: 100%;*/
  /*height: 130px;*/
  /*border-bottom: 1px solid #e7e7e7;*/
/*}*/

/*.good li{*/
  /*float: left;*/
  /*height: 100%;*/
/*}*/
/*.good .good_inf{*/
    /*width: 190px;*/
/*}*/
/*.good .good_inf .good_image{*/
    /*width: 80px;*/
    /*height: 80px;*/
    /*margin-top: 20px;*/
    /*float: left;*/
/*}*/

/*.good .good_inf .good_image img{*/
  /*width: 100%;*/
  /*vertical-align: top;*/
/*}*/

/*.good .good_inf .good_name{*/
  /*margin: 20px 0 0 10px;*/
  /*line-height: 18px;*/
  /*width: 100px;*/
  /*float: left;*/
/*}*/

/*.good .good_price{*/
  /*width: 65px;*/
/*}*/

/*.good .good_price price{*/
  /*margin-top: 20px;*/
  /*line-height: 18px;*/
  /*font-family: Verdana,Tahoma,arial;*/
  /*color: #3c3c3c;*/
  /*font-weight: bold;*/
/*}*/

/*.good .good_s_volume{*/
  /*width: 60px;*/

/*}*/
/*.good .good_s_volume volume{*/
  /*line-height: 18px;*/
  /*margin-top: 20px;*/
  /*font-family: Verdana,Tahoma,arial;*/
  /*color: #900;*/
  /*font-weight: bold;*/
/*}*/

/*.good .good_score{*/
  /*width: 60px;*/
/*}*/

/*.good .good_score .score{*/
  /*line-height: 18px;*/
  /*margin-top: 20px;*/
  /*font-family: Verdana,Tahoma,arial;*/
  /*color: #900;*/
  /*font-weight: bold;*/
/*}*/
</style>
