<template>
    <div class="pos">
        <el-row class="pos-wrap">
            <el-col :span="7" class="order-table">
                <el-tabs>
                    <el-tab-pane label="点餐">
                        <el-table 
                            :data="orderFoodData"
                            border
                            style="width:100%;">
                            <el-table-column
                                label="商品"
                                align="center"
                                prop="goodsName"
                            ></el-table-column>        
                            <el-table-column
                                label="量"
                                align="center"
                                prop="goodsCount"
                                width="55"
                            ></el-table-column>        
                            <el-table-column
                                label="单价"
                                align="center"
                                width="80"
                                prop="goodsPrice"
                            ></el-table-column>        
                            <el-table-column
                                label="操作"
                                align="center"
                                width="130"
                                fixed="right"
                            >
                                <template slot-scope="scope">
                                    <el-button type="text" v-on:click="addCount(scope.row)">新增</el-button>
                                    <el-button type="text" v-on:click="deleteGoods(scope.row)">删除</el-button>
                                </template>
                            </el-table-column>        
                        </el-table>
                            <div class="menu-sum">
                                <small>数量：</small>{{totalCount}}
                                <small>总计：</small>{{totalMoney}}元
                            </div>
                            <div class="order-menu-toolbar">
                                <el-button type="info" size="medium">挂单</el-button>
                                <el-button type="danger" size="medium" @click="deleteAllGoods">删除</el-button>
                                <el-button type="success" size="medium" v-on:click="checkout">结账</el-button>
                                <el-dialog
                                    title="提示信息"
                                    :visible.sync="deleteAllTips"
                                    width = "30%"
                                >
                                    <span>确定删除所有商品？</span>
                                    <span slot="footer" class="dialog-footer">
                                        <el-button @click="deleteAllTips=false;">取消</el-button>
                                        <el-button v-on:click="confirmDeleteAll">确定</el-button>
                                    </span>
                                </el-dialog>
                            </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">挂单</el-tab-pane>
                    <el-tab-pane label="外卖">外卖</el-tab-pane>
                </el-tabs>
            </el-col>
            <el-col :span="17">
                <div class="common-goods">
                    <div class="common-tit">
                        常用商品
                    </div>
                    <div class="common-content">
                        <ul class="clearfix">
                            <li v-for="item in commonGoodsData" v-on:click="addGoods(item)">
                                {{item.goodsName}}<span class="common-price">￥{{item.goodsPrice}}元</span>
                            </li> 
                        </ul>
                    </div>
                </div>
                <div class="other-choice">
                    <el-tabs>
                        <el-tab-pane label="汉堡">
                            <ul>
                                <li v-for="item in type0GoodsData" @click="addGoods(item)">
                                    <div class="pic">
                                        <img :src="item.goodsImg">
                                    </div>
                                    <div class="text">
                                        <p>{{item.goodsName}}</p>
                                        <p>￥{{item.goodsPrice}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="小食">
                             <ul>
                                <li v-for="item in type1GoodsData" @click="addGoods(item)">
                                    <div class="pic">
                                        <img :src="item.goodsImg">
                                    </div>
                                    <div class="text">
                                        <p>{{item.goodsName}}</p>
                                        <p>￥{{item.goodsPrice}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="饮料">
                             <ul>
                                <li v-for="item in type2GoodsData" @click="addGoods(item)">
                                    <div class="pic">
                                        <img :src="item.goodsImg">
                                    </div>
                                    <div class="text">
                                        <p>{{item.goodsName}}</p>
                                        <p>￥{{item.goodsPrice}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="套餐">
                             <ul>
                                <li v-for="item in type3GoodsData" @click="addGoods(item)">
                                    <div class="pic">
                                        <img :src="item.goodsImg">
                                    </div>
                                    <div class="text">
                                        <p>{{item.goodsName}}</p>
                                        <p>￥{{item.goodsPrice}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                    </el-tabs>
                </div>
            </el-col>
        </el-row>
    </div>
</template>
<script>
// api 是一个对象，也可以理解为一个命名空间
import axios from 'axios';
import api from '@/assets/common_api';
    export default {
        data() {
            return {
                orderFoodData:[],
                commonGoodsData: [],

                type0GoodsData:[],
                type1GoodsData:[],
                type2GoodsData:[],
                type3GoodsData:[],

                totalCount: 0,
                totalMoney: 0,

                deleteAllTips: false,
                timer: null
            }
        },
        created() {
            // 1. 发送 ajax 请求常用商品数据
            axios.get(api.commonGoodsApi)
                .then(res => {
                    this.commonGoodsData = res.data;
                })
                .catch(err => {
                    console.log(err);
                })
            // 2. ajax 请求 分类商品数据
            axios.get(api.typeGoodsApi)
                .then(res => {
                    this.type0GoodsData = res.data[0];
                    this.type1GoodsData = res.data[1];
                    this.type2GoodsData = res.data[2];
                    this.type3GoodsData = res.data[3];
                })
                .catch(err => {
                    console.log(err);
                });
        },
        methods: {
            // 新增商品到 orderTable
            addGoods(goodsItem) {
                let isExist = false;
                for (let i = 0; i < this.orderFoodData.length; i++) {
                    if (this.orderFoodData[i].goodsId === goodsItem.goodsId) {
                        isExist = true;
                        // 2. 存在增加数量
                        this.orderFoodData[i].goodsCount++;
                        break;
                    }
                }
                // 1. 没有的时候新增
                if (!isExist) {
                    this.orderFoodData.push({
                        goodsName: goodsItem.goodsName,
                        goodsId: goodsItem.goodsId,
                        goodsPrice: goodsItem.goodsPrice,
                        goodsCount: 1
                    });
                }
                // 增加完计算下 total
                this.getTotal();
            },
            addCount(row) {
                // 当前商品的数量增加
                //  这里竟然是一个指向关系
                //  这里就不需要再进行循环订单数据对比goodsId 了
                row.goodsCount++;
                // 增加完计算下 total
                this.getTotal();
            },
            deleteGoods(row) {
                // 删除当前商品  
                // 过滤出 id 不同的元素
                this.orderFoodData = this.orderFoodData.filter(element => element.goodsId != row.goodsId);
                // 删除完计算下 total 
                this.getTotal();
            },
            getTotal() {
                this.totalCount = 0;
                this.totalMoney = 0;
                this.orderFoodData.forEach(ele => {
                    this.totalCount += ele.goodsCount;
                    this.totalMoney += ele.goodsCount * ele.goodsPrice;
                });
            },
            deleteAllGoods() {
                // 删除所有商品
                // 1. 信息提示
                if (this.orderFoodData.length !== 0) {
                    this.deleteAllTips = true;
                }
            },
            confirmDeleteAll() {
                this.orderFoodData = [];
                this.totalMoney = 0;
                this.totalCount = 0;
                this.deleteAllTips = false;
            },
            checkout(){
                // 结账
                // 防止重复点击，1s 内重复点击无效
                if (this.timer !== null) {
                    clearTimeout(this.timer);
                }
                this.timer = setTimeout( _ => {
                    if (this.orderFoodData.length !== 0){
                        // 可以结账
                        this.totalMoney = 0;
                        this.totalCount = 0;
                        this.orderFoodData = [];
                        this.$message.success('结账成功，谢谢您的照顾！');
                    } else {

                        this.$message({
                            message: '请先选购商品，老板明白您的急切！',
                            type: 'error'
                        });
                    }
                    this.timer = null;
                },500);   
            }
        }
    }
</script>
<style lang="scss" scoped  type="text/css">
.pos {
    width: 95%;
    float: left;
    &,
    .pos-wrap,
    .order-table {
        height: 100%;          
    }

    .order-table {
        background-color: #F9FAFC;
        border-right: 1px solid #C0CCDA;
    }

    .order-menu-toolbar {
        margin-top: 10px;
    }

    .menu-sum {
        text-align: right;
        padding: 10px;
        border-bottom: 1px solid #E5E9F2;
        background-color: #fff;
        font-weight: 700;
        
        small {
            font-weight: normal;

            &:nth-child(2) {
                margin-left: 20px;
            }
        }
    }

    .common-goods {
        
        font-size: 12px;
        border-bottom: 1px solid #E5E9F2;

        .common-tit {
            text-align: left;
            padding: 13px 0px 12px 10px;
            border-bottom: 1px solid #E5E9F2;
        }
        
        .common-content  {

                padding: 0 40px 10px;
            
            li {
                padding: 10px;
                border: 1px solid  #E5E9F2;
                float: left;
                margin-left: 10px;
                margin-top: 10px;
                cursor: pointer;
            }

            .common-price {
                color: #58B7FF;
                padding-left: 5px;
            }
        }
    }

    .other-choice {
        background-color: #f7f7f7;
        padding-left: 40px;

        li {
            float: left;
            width: 23%;
            padding: 5px;
            background-color: #fff;
            border: 1px solid #E5E9F2;
            margin-left: 10px;
            margin-top: 10px;
            cursor: pointer;
        }

        .pic {
            width: 40%;
            float: left;
        }
        
        .text {
            width: 60%;
            float: left;
            text-align: left;
            
            p {
                padding-left: 10px;

                &:first-child {
                    font-size: 18px;
                    color: brown;
                }   
                &:last-child {
                    margin-top: 10px;
                    font-size: 16px;
                }
            }
        }

        img {
            width: 100%;
        }
    }


}

</style>
