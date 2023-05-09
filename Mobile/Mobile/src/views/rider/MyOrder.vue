<template>
    <div>
        <h4></h4>
        <van-card v-for="(order,index) in myOrders" :key="index">
            <template #title>
                <div style="width: 200px">
                    <h3 class="line-limit-length" style="margin: 5px;width: 90px;display: inline-block;text-align: left;">{{order.shop.shopName}}</h3>
                    <span style="margin: 5px;float: right" v-if="order.order.orderStat===3">
                            
                    </span>
                    <span style="margin: 5px;float: right" v-if="order.order.orderStat===4">
                            
                    </span>
                </div>
            </template>
            <template #num>
                <span style="font-size: 15px;color: crimson">${{order.order.deliveryPrice}}</span>
            </template>
            <template #thumb>
                <img :src="order.shop.logoSrc" style="width: 90px" alt=""/>
            </template>
            <template #desc>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{order.shop.addressDetail}}</span>
                </div>
                <div style="text-align: left;font-size: 12px;margin-left: 10px;margin-bottom: 10px">
                    <span>：{{order.address.address}}</span>
                </div>
                <div style="text-align: left;font-size: 13px;margin-left: 10px">
                    <span>：{{order.order.createTime}}</span>
                </div>
                <div style="text-align: left;font-size: 15px;margin-left: 10px" v-if="order.order.orderStat === 3">
                    <span>：</span>
                </div>
                <div style="text-align: left;font-size: 15px;margin-left: 10px" v-if="order.order.orderStat === 4">
                    <span>：</span>
                </div>
            </template>
            <template #footer>
                <van-button round hairline style="width: 80px" size="small" type="primary" @click="deliverySuccess(order)" v-if="order.order.orderStat === 3"></van-button>
                <van-button round hairline style="width: 80px" size="small" type="info" @click="orderInfo(order)"></van-button>
            </template>

        </van-card>
        <div v-if="myOrders.length === 0">
            <van-empty description="~~！！" />
        </div>
        <!---->
        <div style="height:50px;display:block;"></div>

    </div>

</template>

<script>
    import { Dialog } from 'vant';
    import { Notify } from 'vant';
    export default {
        name: "MyOrder",
        created(){
          this.deliveryInfo = this.$store.getters.getRider;
          this.axios.get("http://localhost:8084/getAllOrderByDid",{
              params:{
                  did: this.deliveryInfo.did
              }
          }).then(resp=>{
              console.log(resp.data);
              this.myOrders = resp.data;
          })
        },
        data(){
            return{
                did:'',
                deliveryInfo:'',
                myOrders:[
                    {
                        /*userName:'lky',
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
                        }*/
                    }
                ]
            }
        },
        methods:{
            //，
            deliverySuccess(order){
                console.log(order);
                Dialog.confirm({
                    title: '',
                    message: '？！',
                }).then(() => {
                        console.log(order);
                        this.axios.get("http://localhost:8084/deliverySuccess",{
                            params:{
                                oid:order.order.oid
                            }
                        }).then(resp => {
                            if (resp.data === "success") {
                                Notify({ type: 'success', message: '' });
                                this.flush();
                            } else {
                                Notify({ type: 'danger', message: '' });
                                this.flush();
                            }
                        })
                    })
                    .catch(() => {
                        Notify({ type: 'warning', message: '' });
                    });
                /*this.$confirm("？！", '', {
                    confirmButtonText: '',
                    cancelButtonText: '',
                    type: 'info'
                }).then(() => {
                    console.log(order);
                    this.axios.get("http://localhost:8084/deliverySuccess",{
                        params:{
                            oid:order.oid
                        }
                    }).then(resp => {
                        if (resp.data === "success") {
                            this.$message({
                                message: "！！！",
                                type: 'success'
                            });
                            this.flush();
                        } else {
                            this.$message({
                                message: "！！！",
                                type: 'warning'
                            });
                            this.flush();
                        }
                    })
                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: ''
                    });
                })*/
            },

            orderInfo(order){
                console.log(order);
                this.$router.push({
                    name:"orderDetail",
                    params:{
                        orderInfo:order
                    }
                })
            },
            //
            flush(){
                this.$router.push({
                    name:"blank_rider",
                    params:{
                        url:"myOrder"
                    }
                })
            }
        }
    }
</script>

<style scoped>
    .line-limit-length {

        overflow: hidden;

        text-overflow: ellipsis;

        white-space: nowrap;

    }
</style>
