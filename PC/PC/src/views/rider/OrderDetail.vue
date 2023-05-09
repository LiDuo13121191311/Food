<template>
    <div>
        <van-nav-bar
                title=""
                left-text=""
                left-arrow
                @click-left="onClickLeft"
        />
        <van-card>
            <template #title>
                <div style="width: 200px">
                    <h3 class="line-limit-length" style="margin: 5px;width: 120px;display: inline-block;text-align: left;">{{orderDetail.shop.shopName}}</h3>
                    <!--<span style="float: left;width: 250px" v-if="orderDetail.order.orderStat===3">
                            
                    </span>
                    <span style="margin: 5px;float: right" v-if="orderDetail.order.orderStat===4">
                            
                    </span>-->
                </div>
            </template>
            <br>
            <template #num>
                <span style="font-size: 15px;color: crimson;">${{orderDetail.order.deliveryPrice}}</span>
            </template>
            <template #thumb>
                <img :src="orderDetail.shop.logoSrc" style="width: 90px" alt=""/>
            </template>
            <template #desc>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{orderDetail.shop.addressDetail}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{orderDetail.shop.phone}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{orderDetail.address.address}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{orderDetail.userName}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{orderDetail.userPhone}}</span>
                </div>
                <div style="text-align: left;font-size: 13px;margin-left: 10px">
                    <span>：{{orderDetail.order.createTime}}</span>
                </div>
                <br>
                <div style="text-align: left;font-size: 13px;margin-left: 10px" v-if="orderDetail.order.orderTime!==null">
                    <span>：{{orderDetail.order.orderTime}}</span>
                </div>
                <br>
                <div style="text-align: left;font-size: 13px;margin-left: 10px" v-if="orderDetail.order.pickTime!==null">
                    <span>：{{orderDetail.order.pickTime}}</span>
                </div>

                <div style="text-align: left;font-size: 13px;margin-left: 10px" v-if="orderDetail.order.arriveTime!==null">
                    <span>：{{orderDetail.order.arriveTime}}</span>
                </div>
                <br>
                <div style="text-align: left;font-size: 15px;margin-left: 10px" v-if="orderDetail.order.orderStat === 3">
                    <span>：</span>
                </div>
                <div style="text-align: left;font-size: 15px;margin-left: 10px" v-if="orderDetail.order.orderStat === 4">
                    <span>：</span>
                </div>

                    <br>

            </template>
            <!--<template #footer>
                <van-button round hairline style="width: 80px" type="small" @click="viewOrderInfo(order)"></van-button>
            </template>-->
        </van-card>
        <!---->
        <div style="height:50px;display:block;"></div>
    </div>
</template>

<script>
    export default {
        name: "OrderDetail",
        created() {
            console.log("");
            console.log(this.$route.params.orderInfo);
            this.orderDetail = this.$route.params.orderInfo;
        },
        data(){
            return{
                orderDetail:{
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
                        orderNumber:'DM202108101934100001',
                        totalPrice:'20'
                    }
                },
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
