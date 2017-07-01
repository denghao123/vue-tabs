<template>
  <div id="app">
    <!-- 头部组件 -->
    <ul class="nav">
        <li v-for="(v,k) in compList">
            <div @click="createTab(k,v.label,v.hasChild)">
                <i :class="'icon-'+k"></i>
                <span>{{v.label}}</span>
            </div>
            <ul>
                <li v-for="v2 in v.child" @click="createTab(v2.key,v2.label)">{{v2.label}}</li>
            </ul>
        </li>
        <span class="demo">Vue+tabs动态组件演示</span>
    </ul>

    <div class="tabs-wrap clearfix">
        <ul class="mt10 clearfix">
            <template v-for="(v,k) in tabItems">
                <li v-if="Object.keys(v).length>0" :class="currentTab==k?'active':''" @click="changeTab(k)">{{v.label}}
                <i class="close" @click.stop="closeTab(k)" title="关闭"></i>
                <i class="reload" @click.stop="reload(k)" title="刷新"></i>
                </li>
            </template>
        </ul>
    </div>

    <keep-alive>
        <component class="tabs-con clearfix" @childcreatetab="createTab" @childclosetab="closeTab" :is="currentTab" :refresh="refresh"></component>
    </keep-alive>
  </div>
</template>

<script>
import lib from '../assets/js/lib.js';
import page1 from './page1.vue';
import page2 from './page2.vue';
import page3 from './page3.vue';
import page4 from './page4.vue';
export default {
    data() {
        return {
            currentTab: "",
            refresh: false,
            compList: {},
            tabItems: {}
        }
        },
        mounted() {

            this.compList = {
                "page1": {
                    label: "首页"
                },
                "page2": {
                    label: "产品",
                    hasChild:true,
                    child:[{
                        label:"安卓",
                        key:"page2"
                    },{
                        label:"苹果",
                        key:"page3"
                    }]
                },
                "page4": {
                    label: "关于"
                },
            }

            // 默认加载首页
            this.createTab("page1","首页");
        },
        methods: {

            reload(key){
                this.currentTab = "page1";
                this.$nextTick(()=>{
                    this.currentTab = key;
                    this.refresh=true
                })
            },

            createTab(key,label,hasChild) {

                if (hasChild) return false;

                if (lib.isEmptyObj(this.tabItems[key])) {
                    let obj = {}
                    obj[key] = {
                        label:label
                    }
                    this.tabItems = Object.assign({}, this.tabItems, obj);
                    this.refresh = true;
                } else {
                    this.refresh = false;
                }
                this.currentTab = key;
            },

            changeTab(key) {
                this.currentTab = key;
                this.refresh=false;
            },

            closeTab(key) {
                if (!key) {
                    this.tabItems = {
                        "page1": {
                            label: "首页"
                        }
                    };
                    this.currentTab = "page1";
                } else {
                    let obj = {}
                    obj[key] = {};
                    this.tabItems = Object.assign({}, this.tabItems, obj);
                    this.currentTab = "page1";
                }
            }
        },
        components: {
            page1,page2,page3,page4
        }
}
</script>

<style lang="scss">
.demo{
    padding: 10px;
    float: right;
    font-size: 24px;
    color: #999;
    vertical-align: middle;
}
.nav{
    background-color: #333;
    >li{
        position: relative;
        cursor: pointer;
        color: white;
        display: inline-block;
        width: 100px;
        height: 50px;
        line-height: 50px;
        text-align: center;
        >ul{
            display: none;
            position: absolute;
            top:50px;
            left:0;
            width: 100%;
            z-index: 2;
            background-color:#444;
            li{
                height: 36px;
                line-height: 36px;
                &:hover{
                    background-color:#666;
                }
            }
        }

        &:hover{
            background-color:#444;
        }
        &:hover >ul{
            display: block;
        }
    }
}

.tabs-wrap{
    padding:20px 10px 0;
    border-bottom:1px solid #d8d8d8;
    li{
        padding: 7px 40px 7px 12px;
        margin:4px 8px 0 8px;
        float: left;
        border:1px solid #ccc;
        position: relative;
        cursor: pointer;
        border-bottom: none;
        border-radius: 4px 4px 0 0;
        background-color: #efefef;
        font-size: 14px;
        i{
            position: absolute;
            top:7px;
            right: 5px;
            width: 14px;
            height: 14px;
            opacity:0.7;
            background:url(../assets/image/icon.png) no-repeat;
            background-size:30px; 
            &:hover{
                opacity:1;
            }
        }
        .close{
            background-position: -15px 0;
        }
        .reload{
            background-position: 0px 0;
            right: 20px;
        }
        &.active{
            color: #20a0ff;
            border-color:#20a0ff;
            background-color:#fff;
        }
        &:first-child{
            padding: 7px 14px 7px 14px;
            .close,.reload{
                display: none;
            }
            .reload{
                right: 4px;
            }
        }
    }
}

.tabs-con{
    padding: 20px;
    color: #333;
}
</style>
