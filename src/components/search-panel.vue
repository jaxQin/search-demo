<template>
    <div class="search-panel">
        <logoSelect @getindex="getIndex"> </logoSelect>
        <div class="search-input">
            <input type="text" v-model="keyword" value="" @keyup="get" @keydown.down="selectDown()" @keydown.up.prevent="selectUp()" @keydown.enter="search()" />
            <span class="search-reset" @click="clearInput()">&times;</span>
            <button class="search-btn" @click="search()" >搜一下</button>
            <div class="search-select">
                <ul v-show="searchData">
                    <li v-for="(item,index) in searchData" class="search-select-option" :class="{active:index==now}" @mouseover="hover(index)" @click="search()">{{item}}</li>
                </ul>
            </div>
        </div>
    </div>
</template>
<script>
var logoData = [{
    name: '360搜索',
    searchSrc: 'https://www.so.com/s?ie=utf-8&shb=1&src=360sou_newhome&q='
}, {
    name: '百度搜索',
    searchSrc: 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd='
}, {
    name: '搜狗搜索',
    searchSrc: 'https://www.sogou.com/web?query='
}]
import logoSelect from './logo-select.vue';

export default {
    name: 'search-panel',
    components: {
        logoSelect
    },
    data() {
        return {
            logoData: logoData,
            searchIndex: 0,
            keyword: "",
            searchData: [],
            now: -1
        }
    },
    methods: {
        //获取当前是什么搜索
        getIndex(i) {
            this.searchIndex = i;
        },
        // 输入的时候自动联想
        get(ev) {
            // 如果按得键是上或者下，就不进行ajax
            if (ev.keyCode == 38 || ev.keyCode == 40) {
                return;
            }
            this.$http.jsonp('https://sug.so.360.cn/suggest?word=' + this.keyword + '&encodein=utf-8&encodeout=utf-8').then(function(res) {
                //                        console.log(res.data.s);
                this.searchData = res.data.s;
                console.log(this.searchData)
            }, function() {

            });
        },
        selectDown() {
            this.now++;
            //到达最后一个时，再按下就回到第一个
            if (this.now == this.searchData.length) {
                this.now = 0;
            }
            this.keyword = this.searchData[this.now];
        },
        selectUp() {
            this.now--;
            //到达最后一个时，再按下就回到第一个
            if (this.now == -1) {
                this.now = this.searchData.length - 1;
            }
            this.keyword = this.searchData[this.now];
        },
        search() {
            //打开对应的搜索界面
            window.open(this.logoData[this.searchIndex].searchSrc + this.keyword);
        },
        hover(i) {
            this.now = i;
        },
        clearInput(){
          this.keyword="";
          this.searchData=[];
        }

    }

}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
ul,
li {
    list-style: none;
    padding: 0;
    margin: 0;
}

.search-panel {
    position: relative;
    width: 600px;
    height: 300px;
    margin: 40px auto;
}

.search-input {
    height: 45px;
    width: 600px;
    margin: 0px auto;
    margin-top: 10px;
    position: relative;
}

.search-input input {
    border: 1px solid #e4e4e4;
    box-sizing: border-box;
    width: 500px;
    height: 45px;
    font-size: 18px;
    float: left;
    padding-left: 10px;
    padding-right: 10px;
    overflow: hidden;
}

.search-btn {
    height: 45px;
    width: 100px;
    border: 1px solid mediumseagreen;
    background-color: mediumseagreen;
    color: white;
    font-size: 16px;
    font-weight: bold;
    float: left;
}

.search-btn {
    cursor: pointer
}

.search-select {
    position: absolute;
    top: 45px;
    width: 500px;
    box-sizing: border-box;
    z-index: 999;
}

.search-select li {
    border: 1px solid #d4d4d4;
    border-top: none;
    border-bottom: none;
    background-color: #fff;
    width: 100%;
    cursor: pointer;
}

.active {
    background-color: #eee !important;
}

.search-select-option {
    box-sizing: border-box;
    padding: 7px 10px;
}

input::-ms-clear {
    display: none
}

.search-reset {
    width: 21px;
    height: 21px;
    position: absolute;
    display: block;
    line-height: 21px;
    text-align: center;
    cursor: pointer;
    font-size: 20px;
    right: 110px;
    top: 12px
}

.search-select-list {
    transition: all 0.5s
}

.itemfade-enter,
.itemfade-leave-active {
    opacity: 0;
}

.itemfade-leave-active {
    position: absolute;
}

[v-cloak] {
    display: none;
}

.selectback {
    background-color: #eee !important;
    cursor: pointer
}

.search-select ul {
    margin: 0;
    text-align: left;
}
</style>
