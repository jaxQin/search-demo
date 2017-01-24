<template>
    <div class="search-logo">
        <img :src="logoList[selectedNow].imgSrc" height="140" width="480" alt="" />
        <span class="arrow-down" @click="toggleLogoList"></span>
        <ul class="logoList" v-show="logoListFlag">
            <li class="logoItem" v-for="(item,index) in logoList" @click="selectLogo(index)"><img :src="item.imgSrc"></li>
        </ul>
    </div>
</template>
<script>
var logoList = [{
    imgSrc: require("../assets/360_logo.png")
}, {
    imgSrc: require("../assets/baidu_logo.png")
}]
export default {
    name: 'logo-select',
    data() {
        return {
            logoListFlag: false,
            selectedNow: 0,
            logoList: logoList
        }
    },
    methods: {
        toggleLogoList() {
            this.logoListFlag = !this.logoListFlag
        },
        selectLogo(index) {
            this.selectedNow = index;
            this.logoListFlag = false;
            this.$emit('getindex', this.selectedNow);
        }
    }

}
</script>
<style>
.search-logo {
    height: 140px;
    position: relative;
}

.arrow-down {
    position: absolute;
    width: 0;
    height: 0;
    border: 8px solid;
    border-color: #000 transparent transparent transparent;
    right: 100px;
    top: 66px;
    cursor: pointer;
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
    outline: none;
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
    top: 12px;
}

.search-btn {
    float: left;
    height: 45px;
    width: 100px;
    border: 1px solid #3CB371;
    background-color: #3CB371;
    color: white;
    font-size: 16px;
    font-weight: bold;
    outline: none;
    cursor: pointer;
}

.logoList {
    position: absolute;
    top: 100%;
    width: 200px;
    left: 50%;
    margin-left: -100px;
    z-index: 999999;
    border: 1px solid #d4d4d4;
}

.logoList li {
    width: 100%;
    height: 80px;
    background-color: #fefefe;
    line-height: 80px;
    padding-top: 1px;
}

.logoList li:hover {
    background-color: #eee !important;
    cursor: pointer;
}

.logoList li img {
    width: 100%;
    margin-top: 10px;
    display: block;
    height: 100%;
}
</style>
