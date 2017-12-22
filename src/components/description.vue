<template>
    <div id="description">
        <div v-if="!this.noBook" class="whole-page">
			<Row>
				<Col span="10">
					<img v-bind:src="itemDetail.picture" v-bind:alt="itemDetail.name" width="300" height="300">
				</Col>
				<Col span="13" offset="1">
                    <p class="book-name">
                        <Tag v-if="itemDetail.isUsedbooks" color="yellow" class="static-tag">二手</Tag>
                        {{ itemDetail.name }}
                    </p>
					<p class="book-author">{{ itemDetail.author }} 著</p>

					<p class="book-price">
						<Row>
							<Col span="5">
								<p style="font-size: 15px">价格</p>
							</Col>
							<Col span="19">
								<p class="price-digit">￥{{ itemDetail.price.toFixed(2) }}
                                    <Tag v-if="itemDetail.freight === 0" color="green" class="static-tag">包邮</Tag>
                                    <span v-else class="book-freight">(另需运费：<span class="freight-digit">￥{{ itemDetail.freight.toFixed(2) }}</span>)</span>
                                </p>
							</Col>
						</Row>
					</p>

					<p class="book-tag">
						<Row>
							<Col span="5">
								<p style="font-size: 15px">月销量</p>
							</Col>
							<Col span="19">
								<p class="price-volume">{{ itemDetail.sales_volume }}</p>
							</Col>
						</Row>
					</p>

					<!--<p class="book-tag">-->
						<!--<Row>-->
							<!--<Col span="5">-->
								<!--<p style="font-size: 15px">来源</p>-->
							<!--</Col>-->
							<!--<Col span="19">-->
								<!--<p class="price-seller">{{ good_source[itemDetail.seller] }}</p>-->
							<!--</Col>-->
						<!--</Row>-->
					<!--</p>-->

					<p class="book-rate">商品评分&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
						<Rate disabled show-text allow-half v-model="itemDetail.scores.good">
						<span style="color: #f5a623; font-size: 17px">{{ itemDetail.scores.good.toFixed(1) }}</span>
						</Rate>
					</p>
					<p class="book-credit">店铺评分&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
						<Rate disabled show-text allow-half v-model="itemDetail.scores.credit">
						<span style="color: #f5a623; font-size: 17px">{{ itemDetail.scores.credit.toFixed(1) }}</span>
						</Rate>
					</p>
                    <div class="book-tags">
                        <span v-for = "tag_id in itemDetail.tags" class = "tag">
                            <Tag>{{tags_inf[tag_id]}}</Tag>
                        </span>
                    </div>
                    <div class="book-purchase">
                        <Row type="flex" align="middle">
                            <Col span="5">
                                <img :src="seller_img[itemDetail.seller]" height="30px" width="70px">
                            </Col>
                            <Col span="10">
                                <div class="book-btn">
                                    <Button type="primary" size="large" icon="ios-cart" @click="openUrl" long>立即购买</Button>
                                </div>
                            </Col>
                        </Row>
                    </div>


				</Col>
			</Row>

			<div class="book-tab">
				<Tabs value="detail">
					<TabPane label="详细信息" name="detail">
						<Row class="set-padding-top20">
							<Col span="5">
								<p>出版社</p>
							</Col>
							<Col span="5">
								<p>{{ itemDetail.detail.publisher }}</p>
							</Col>
							<Col span="5" offset="4">
								<p>出版日期</p>
							</Col>
							<Col span="5">
								<p>{{ itemDetail.detail.pub_date }}</p>
							</Col>
						</Row>

						<Row class="set-padding-top20">
							<Col span="5">
								<p>页数</p>
							</Col>
							<Col span="5">
                <p v-if="itemDetail.detail.pages <= 0">未知</p>
								<p v-else>{{ itemDetail.detail.pages }}</p>
							</Col>
							<Col span="5" offset="4">
								<p>语言</p>
							</Col>
							<Col span="5">
								<p>{{ itemDetail.detail.language }}</p>
							</Col>
						</Row>

						<div class="book-description">
							<Card>
								<p slot="title">简介</p>
								<p>{{ itemDetail.information.introduction }}</p>
							</Card>
						</div>

						<div class="book-content">
							<Collapse>
								<Panel name="1">目录
                  <p slot="content"><pre>{{ itemDetail.information.content }}  </pre></p>
								</Panel>
							</Collapse>
						</div>
					</TabPane>

					<TabPane label="用户评价" name="comment">
						<div v-if="total_comments > 0" class="comment-list" v-for="item in showing_comments">
							<Card>
								<p slot="title">{{ item.user }}</p>
								<p>{{ item.comment }}</p>
							</Card>
						</div>
						<div v-else class="no_comment">暂无评论</div>
						<div class="page-switch">
							<Page :total="total_comments" :page-size="page_comments" show-total @on-change="choose_page"></Page>
						</div>
					</TabPane>
				</Tabs>
			</div>
        </div>
        <div v-else class="no-book">
            <p>未找到指定图书</p>
        </div>

    </div>
</template>

<script>
    export default {
        props: ['itemDetail'],
        mounted() {
            this.choose_page(1);
        },
        data: function () {
            return {
                noBook: false,
                good_source: ["淘宝", "京东", "天猫", "亚马逊", "当当"],
                total_comments: this.itemDetail.comments.length,
                showing_comments: [],
                current_page: 1,
                page_comments: 5,
                tags_inf: ["正品保证","极速退款","七天退换","有赠品"],
                seller_img: ["https://img.alicdn.com/tfs/TB1_uT8a5ERMeJjSspiXXbZLFXa-143-59.png",
                    "https://img14.360buyimg.com/da/jfs/t7366/203/1731206510/2597/d71c891f/59a056c1N5e4d6940.png",
                    "https://img.alicdn.com/tfs/TB1MaLKRXXXXXaWXFXXXXXXXXXX-480-260.png",
                    "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=3455760616,3212888784&fm=27&gp=0.jpg",
                    "https://login.dangdang.com/images/logo.png"],
            }
        },
        methods: {
            openUrl: function () {
//                alert(typeof this.itemDetail);
                window.open(this.itemDetail.url);
            },
            choose_page: function (page) {
                if (this.total_comments <= this.page_comments * page) {
                    this.showing_comments = this.itemDetail.comments.slice((page - 1) * this.page_comments, this.total_comments);
                } else {
                    this.showing_comments = this.itemDetail.comments.slice((page - 1) * this.page_comments, page * this.page_comments);
                }

            }
        },
        watch: {
            itemDetail: function (new_obj) {
                if (new_obj === undefined || Object.keys(new_obj).length === 0) {
                    this.noBook = true;
                } else {
                    this.noBook = false;
                    this.total_comments = new_obj.comments.length;
                    this.choose_page(1);
                }
            }
        }
    }


</script>

<style>

    /*#description {*/
        /*height: 600px;*/
        /*overflow-x: hidden;*/
        /*overflow-y: hidden;*/
    /*}*/

    .book-name {
        font: bold 30px "arial";
        color: #3c3c3c
    }

    .book-author {
        font: 15px "arial";
        color: #3399ff;
        padding-top: 10px;
    }

    .book-price {
        padding-top: 20px !important;
    }

    .price-digit {
        font: bold 20px "verdana";
        color: orangered;
        letter-spacing: 1px;
    }

    .freight-digit {
        font: bold 12px "verdana";
        color: orangered;
    }

    .book-tag {
        padding-top: 8px;
        font-size: 15px
    }

    .book-rate {
        padding-top: 7px;
        font-size: 15px;
    }

    .book-credit {
        padding-top: 2px;
        font-size: 15px;
    }

    .book-btn {
        /*padding-top: 17px;*/
    }

    .book-tab {
        padding: 30px 20px 20px 20px;
    }

    .set-padding-top20 {
        padding-top: 20px;
    }

    .book-description {
        padding-top: 30px;
    }

    .book-content {
        padding-top: 10px;
    }

    .comment-list {
        padding-top: 15px;
    }

    .page-switch {
        padding: 30px 0;
        text-align: center;
    }

    .static-tag {
        cursor: auto;
    }

    .book-freight {
        font-size: 10px;
        color: #aaaaaa
    }

    .book-tags {
        padding: 10px 0;
    }

    .book-purchase {
        padding: 5px 0;
    }

</style>
