<template>
    <div id="app">
        <div class="top-pin">
            <Search id="search_box" v-on:sendResult="getResult"></Search>
        </div>

        <div class="page-padding">
            <div class="page_body">
                <Row>
                    <Col span="7">
                        <GoodList id="good_list" v-bind:search-arr="search_result"
                        v-on:sendItem="getItem"></GoodList>
                    </Col>

                    <Col span="16" offset="1">
                        <Description id="description" v-bind:item-detail="book_info"></Description>
                    </Col>
                </Row>

            </div>
        </div>

        <div class="bottom-pic">
            <Footer id="bottom-show"></Footer>
        </div>

    </div>
</template>

<script>
    import Search from './components/Search.vue';
    import GoodList from './components/GoodList.vue';
    import Description from './components/description.vue';
    import Footer from './components/footer.vue';

    export default {
        name: 'app',
        data: function() {
            return {
                search_result: [10000, 10001, 10002, 10003, 10004, 10005],
                book_info: {
                    "id": -1,
                    "isUsedbooks": -1,
                    "name": "",
                    "price": -1,
                    "freight": -1,
                    "sales_volume": -1,
                    "author": "",
                    "seller": -1,
                    "tags": [],
                    "url": "",
                    "picture": "",
                    "detail": {
                    "publisher": "",
                        "pub_date": "",
                        "pages": -1,
                        "language": ""
                    },
                    "information": {
                        "introduction": "",
                            "content": ""
                    },
                    "scores": {
                        "credit": -1,
                            "good": -1
                    },
                    "comments": [{
                        "user": "",
                        "comment": ""
                    }
                    ]
                }
            }
        },
        methods: {
            getResult: function (m_search_result) {
                this.search_result = m_search_result;
            },
            getItem: function (m_detail) {
                this.book_info = m_detail;
            }
        },
        components: {
            Search,
            GoodList,
            Description,
            Footer
        }
    }
</script>

<style>
    #app {
        background: #eeeeee;
    }

    .page-padding {
        padding-top: 30px;
        padding-bottom: 20px;
    }

    .page_body {
        margin: 0 auto;
        max-width: 1350px;
        min-width: 1100px;
        /*border-radius: 10px;*/
        /*padding: 40px 30px 30px 30px;*/
        /*background-color: #ffffff*/
    }

    #search_box {
        width: 100%;
        margin: 0 auto;
        background-color: #ffffff
    }

    .top-pin {
        text-align: center;
        /*padding-top: 25px;*/
    }

    #good_list {
        /*border-right: 1px solid #000000;*/
    }

    #description {
        -webkit-border-radius: 8px;
        -moz-border-radius: 8px;
        border-radius: 8px;
        padding: 40px 30px;
        background: #ffffff;

    }

    .bottom-pic{
        padding-bottom:0;
    }

    #bottom-show{
        width:100%;
        height:150px;
        background:#ffffff;
    }
    #comment_list {

    }
</style>
