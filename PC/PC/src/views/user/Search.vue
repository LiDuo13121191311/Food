<template>
    <div>
        <van-nav-bar
                title=""
                left-text=""
                left-arrow
                @click-left="onClickLeft"
        />
        <!-- <van-search v-model="value" placeholder="" show-action>
            <template #action>
                <div @click="search"></div>
            </template>
        </van-search> -->
        <el-card class="box-card" v-show="cardShow" style="background-color: #f7f8fa">
            <div slot="header" class="clearfix">
                <span>{{title}}</span>
            </div>
            <!--<van-card  v-for="(good,index) in goods" :key="index"
                       :num="good.num"
                       :price="good.price"
                       :desc="good.desc"
                       :title="good.title"
                       :thumb="good.thumb"
            >
            </van-card>-->
            <ul>
                <li v-for="(shop,index) in shops" :key="index">
                    <div style="border-radius: 10px;background-color: white"  @click="toShop(shop)">
                        <div style="display:inline-block;float: left">
                            <img :src="shop.logoSrc" width="80" height="80" alt="">
                        </div>
                        <div style="">
                            <h3 style="width: 195px;margin-bottom: 5px;padding-left: 80px;text-align: left">{{shop.shopName}}</h3>
                            <div style="width: 195px;padding-left: 80px;text-align: left">
                                <span style="font-size: 15px;">Monthly sale{{shop.totalSales}}</span><br>
                            </div>
                            <!-- <div style="width: 195px;font-size: 15px;padding-left: 80px;text-align: left">
                                <span style="">${{shop.deliveryCost}}</span>
                                <span style="margin: 5px">${{shop.minCost}}</span>
                                <span style="margin: 5px" v-if="shop.deliveryTime===null">40minute</span>
                                <span style="margin: 5px" v-if="shop.deliveryTime!==null">{{shop.deliveryTime}}minute</span>
                                <br>
                            </div> -->
                            <p style="width: 70px;margin: 10px"></p>
                            <p style="width: 250px;height: 50px;margin: 10px" v-if="shop.desc!==null">{{shop.desc}}</p>
                            <p style="width: 250px;height: 50px;margin: 10px" v-if="shop.desc===null"></p>
                        </div>
                    </div>

                </li>
            </ul>
        </el-card>
        <!---->
        <div style="height:60px;display:block;background-color: white"></div>
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "Search",
        data(){
            return{
                totalPrice:null,
                show:true,
                value:'',
                cardShow:true,
                title:'',
                shops:[
                    {
                        shopName:"ice",
                        desc:"ice",
                        totalSales: 2000,
                        minCost:"15",
                        deliveryCost:"3",
                        deliveryTime:"40",
                        logoSrc:"upload/icelogo_20210905175221.png"
                    },

                ],
            }
        },
        methods: {
            onClickLeft(){
                this.$router.push({
                    path:'/home_user',
                });

            },

            search(){
                console.log(this.value);
                this.axios.get("http://localhost:8084/findGoodsAndShopsByValue",{
                    params:{
                        value:this.value
                    }
                }).then(resp=>{
                    console.log(resp.data);
                    this.shops = resp.data;
                })
            },

            /**/
            toShop(shop){
                console.log(""+shop.sid);
                this.$router.push({
                    name:'shop',
                    params:{
                        shop:shop
                    }
                })
            },


        }
    }
</script>

<style scoped>

</style>
