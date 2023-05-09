<template>
    <div>
        <van-nav-bar
                title=""
                left-text=""
                left-arrow
                @click-left="onClickLeft"
        />
        <!--<van-card v-model="orderInfo" :key="index">
            <template #title>
                <div style="width: 200px">
                    <h3 class="line-limit-length" style="margin: 5px;width: 90px;display: inline-block;text-align: left;">{{shop.shopName}}</h3>
                    <span style="margin: 5px;float: right" v-if="order.orderStat===3">
                            
                    </span>
                    <span style="margin: 5px;float: right" v-if="order.orderStat===4">
                            
                    </span>
                </div>
            </template>
            <template #num>
                <span style="font-size: 15px;color: crimson;">${{order.deliveryPrice}}</span>
            </template>
            <template #thumb>
                <img :src="shop.logoSrc" style="width: 90px" alt=""/>
            </template>
            <template #desc>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{shop.address}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{shop.phone}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{address.address}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{userName}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{userPhone}}</span>
                </div>
                <div style="text-align: left;font-size: 13px;margin-left: 10px">
                    <span>：{{order.createTime}}</span>
                </div>
                <br>
                <div style="text-align: left;font-size: 13px;margin-left: 10px" v-if="order.orderTime!==null">
                    <span>：{{order.orderTime}}</span>
                </div>
                <div style="text-align: left;font-size: 13px;margin-left: 10px" v-if="order.pickTime!==null">
                    <span>：{{order.pickTime}}</span>
                </div>
                <div style="text-align: left;font-size: 13px;margin-left: 10px" v-if="order.arriveTime!==null">
                    <span>：{{order.arriveTime}}</span>
                </div>
                <div style="text-align: left;font-size: 15px;margin-left: 10px" v-if="order.orderStat === 3">
                    <span>：</span>
                </div>
                <div style="text-align: left;font-size: 15px;margin-left: 10px" v-if="order.orderStat === 4">
                    <span>：</span>
                </div>

                    <br>

            </template>
            &lt;!&ndash;<template #footer>
                <van-button round hairline style="width: 80px" type="small" @click="viewOrderInfo(order)"></van-button>
            </template>&ndash;&gt;
        </van-card>-->
        <!---->
        <div style="height:50px;display:block;"></div>

    </div>

</template>

<script>
    export default {
        create(){
            console.log("");
            //console.log(this.$route.params.orderInfo)
            /*this.axios.get("http://localhost:8084/getOrderByDid",{
                params:{
                    did: this.deliveryInfo.did
                }
            }).then(resp=>{
                console.log(resp.data);
                this.myOrders = resp.data;
            })*/
        },
        data(){
            return{
                /*orderInfo:[
                    {
                        userName:'lky',
                        userPhone:'12112112112',
                        shop:{
                            address:'KFC',
                            logoSrc:'/pic/kfcLogo.png',
                            shopName:'KFC',
                            phone:'12313212312'
                        },
                        address:{
                            address:' 138  7  501 '
                        },
                        order:{
                            orderStat:3,
                            deliveryPrice:'5',
                            createTime:'2021-8-10 19:34:10',
                            orderTime:null,
                            pickTime:null,
                            arriveTime:null,
                            orderNumber:'DM202108101934100001',
                            totalPrice:'20'
                        }
                    }
                ]*/
            }
        },
        methods:{
            onClickLeft(){
                this.$router.push("/myOrder");
            }
        }
    }
</script>

<style scoped>

</style>
