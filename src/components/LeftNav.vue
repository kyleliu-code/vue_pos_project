<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="iconfont icon-shouyinzhongxin"></i>
                <p>收银</p>
            </li>
            <li>
                <i class="iconfont icon-dianpu"></i>
                <p>店铺</p>
            </li>
            <li>
                <i class="iconfont icon-shangpin"></i>
                <p>商品</p>
            </li>
            <li>
                <i class="iconfont icon-huiyuan"></i>
                <p>会员</p>
            </li>
            <li>
                <i class="iconfont icon-tongji"></i>
                <p>统计</p>
            </li>
            <li>
                <i class="iconfont icon-tubiaolunkuo-"></i>
                <p>设置</p>
            </li>
        </ul>
    </div>
</template>
<script>
    export default {
        data() {
            return {}
        }

    }
</script>
<style lang="scss" scoped>
    .left-nav {
        width: 5%;
        float: left;
        background-color: #1D8ce0;
        color: #fff;
        font-size: 10px;
        height: 100%;
        text-align: center;
        
        .iconfont {
            font-size: 24px;
        }

        li {
            padding: 10px;
            border-bottom: 1px solid #20a0ff;
            cursor: pointer;
        }

    }
</style>