<template>
    <div id="page-footer">
        <div class="nav-footer">
            <Row type="flex" justify="center">
                <i-col span="6">
                    <p class="icon-title">
                      <Icon type="wand" size="20" color="#2d8cf0"></Icon>
                      相关推荐
                    </p>
                    <p>Vue快速入门</p>
                    <a href="javascript:void(0)" v-on:click="iview_show">iView - A UI Toolkit based on Vue.js</a>
                    <p>HTML5+CSS教程</p>
                    <p>jQuery教程</p>
                    <p>JSON文件介绍</p>
                </i-col>
                <i-col span="6">
                    <p class="icon-title">
                       <Icon type="social-github" size="20" color="#2d8cf0"></Icon>
                       GitHub
                    </p>
                    <p><a href="javascript:void(0)" v-on:click="git1">FPSEI-group2-dev---master</a></p>
                    <p><a href="javascript:void(0)" v-on:click="git2">FPSEI-group2-dev---dev</a></p>
                    <p><a href="javascript:void(0)" v-on:click="git3">FPSEI-group2-dev---feature</a></p>
                </i-col>
                <i-col span="2">
                    <p class="icon-title">
                       <Icon type="chatbubbles" size="20" color="#2d8cf0"></Icon>
                       社区
                    </p>
                    <p>使用教程</p>
                    <p>用户体验</p>
                    <p>关于我们</p>
                </i-col>
            </Row>

        </div>
    </div>
</template>

<script>
    export default{
        name:'footer',
        data:function(){
            return{
                pos1:"https://github.com/yfzm/FPSEI-group2/tree/master",
                pos2:"https://www.iviewui.com/",
                pos3:"https://github.com/yfzm/FPSEI-group2/tree/dev",
                pos4:"https://github.com/yfzm/FPSEI-group2/tree/dev"
            }
        },
        methods:{
            git1:function(){
                window.open(this.pos1);
            },
            git2:function(){
                window.open(this.pos3);
            },
            iview_show:function(){
                window.open(this.pos2);
            },
            git3:function(){
                window.open(this.pos4);
            }
        }

    }
</script>

<style>
    .nav-footer{
        padding:13px 0;
        /*padding-left:180px;*/
    }
    .icon-title{
        padding-bottom: 10px;
        font-size:15px;
        font-weight:bold;
        color:#80848f;
    }


</style>
