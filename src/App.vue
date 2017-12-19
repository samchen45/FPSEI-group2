<template>
    <div id="app">
        <Affix class="top-pin">
            <Search id="search_box" v-on:sendResult="getResult"></Search>
        </Affix>

        <div class="page_body">
            <Row>
                <Col span="9">
                    <GoodList id="good_list" v-bind:search-arr="search_result"
                                             v-on:sendItem="getItem"></GoodList>
                </Col>

                <Col span="14" offset="1">
                    <Description id="description" v-bind:item-detail="book_info"></Description>
                </Col>
            </Row>

        </div>

    </div>
</template>

<script>
    import Search from './components/Search.vue';
    import GoodList from './components/GoodList.vue';
    import Description from './components/description.vue';

    export default {
        name: 'app',
        data: function() {
            return {
                search_result: [10000, 10001, 10002, 10003, 10004],
//                book_info: {}
                book_info: {
                    "id": 10000,
                    "name": "Algorithms",
                    "price": 31.60,
                    "sales_volume": 50,
                    "author": "Sanjoy",
                    "seller": 1,
                    "url": "http://item.jd.com/10079570.html",
                    "picture": "http://img10.360buyimg.com/n5/jfs/t3910/74/286809770/130084/6c02e960/584778f9N4cb292f8.jpg",
                    "detail": {
                        "publisher": "清华大学出版社",
                        "pub_date": "2008-07-01",
                        "pages": 345,
                        "language": "Chinese"
                    },
                    "information": {
                        "introduction": "作为一本介绍算法技术和思想的书籍，本书不仅是面各信息学科大学生的优秀教材（或参考书），更是将任何具有初等数学基础的人引入算法应用与研究殿堂的一块引路石。本书循序渐进、深入浅出地展示了算法研究与应用领域中，从模型分析、算法构造到复杂性分析和算法优化的方方面面。涉及内容从古老的算术算法、排序算法、简单算法、线性规划、动态规划、随机算法以及NP复杂理论，甚至是尚未完全显现全貌的量子计算，覆盖了经典、现代和未来算法发展的众多代表性成果。　　《国外经典教材·计算机科学与技术：算法概论》选材新颖，内容丰富，适用于作为计算机学科以及相关学科算法课程的教材和参考书，同时也可作从事算法研究的入门书籍。",
                        "content": "第0章 序言\n0.1 书籍和算法\n0.2 从Fibonacci数列开始\n0.3 大O符号\n习题"
                    },
                    "scores": {
                        "credit": 4.5,
                        "good": 4.7
                    },
                    "comments": [
                        {
                            "user": "Jay",
                            "comment": "Very good!!!"
                        },
                        {
                            "user": "LIUok",
                            "comment": "cheap and good use."
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
            Description
        }
    }
</script>

<style>
    #app {
        /*width: 100%;*/
    }

    .page_body {
        margin: 0 auto;
        max-width: 1300px;
        padding-top: 120px;

    }

    #search_box {
        width: 800px;
        margin: 0 auto;
    }

    .top-pin {
        text-align: center;
        /*padding-top: 25px;*/
    }

    #good_list {

    }

    #description {

    }

    #comment_list {

    }
</style>
