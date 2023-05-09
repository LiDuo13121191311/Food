<template>
    <div>
    <div v-if="showShopInfo" style="width: 60%;margin-left: 20%;">
        <!--、-->
        <span style="margin-top: 10px;font-size: 25px; position: absolute;z-index: 10;color: #fff;">
                    <van-icon name="arrow-left" @click="back"/>
        </span>
        <!-- <span style="margin-top: 10px;right: 10px; font-size: 25px; position: absolute;z-index: 10;">
            <van-icon name="search" @click="searchGoods"/>
        </span> -->
        <!-- <van-image width="100%" height="150" :src="shopMessage.logoSrc" style="position: relative"/> -->
        <img class="User_content_swipe" style="width: 100%;height: 150px;" src="../../assets/back.jpg" />

        <!--logo-->
        <div style="top:80px;left: 30%;width: 40%;height: 100px;box-shadow:rgb(0 0 0 / 50%) 1px 1px 11px 2px;border-radius:10px;background-color: white;position: absolute;z-index: 10">
            <!-- <div style="width: 200px;display: inline-block;float: left">
                <van-image width="170" height="100" :src="shopMessage.logoSrc" round/>
            </div> -->
            <div style="width: 100%;display: inline-block;margin-left: 5px;margin-top:5px;float: left">
                <h3 style="margin: 0">{{shopMessage.shopName}}</h3>
                <h6 style="margin: 10px 0;">Monthly sale{{shopMessage.totalSales}}</h6>
                <!-- <h6 style="margin: 0">{{shopMessage.deliveryTime}}minute</h6> -->
                <span style="margin: 0;font-size: 5px">Notice:{{shopMessage.desc}}</span>
            </div>
            <!--<div style="width:100px">
                <h5>：{{shopMessage.totalSales}}</h5>
            </div>-->
        </div>
        <div style="margin-top: 60px;">
            <!---->
            <van-tabs v-model="activeName" @click="hide(activeName)" style="margin: 0 17%;">
                <van-tab title="order" name="good" style="color: #409EFF;margin-left: 0;">
                    <el-tabs tab-position="left" style="width:100%;margin-top: 0; margin-bottom:0;height: 100%;">
                        <template v-for="(item,index1) in goodList" >
                            <el-tab-pane :label="item.categoryName" type="border-card">
                                <ul>
                                    <li v-for="(good,index2) in item.goods" :key="index2">
                                        <div style="border-radius: 10px;background-color: white;display: flex;">
                                            <div style="display:inline-block;">
                                                <img :src="Phototimg(good.goodPic)" width="100" height="100" alt="">
                                            </div>
                                            <div style="width: 100%;">
                                                <div style="font-size: 18px;font-weight: bold;text-align: center;">{{good.goodName}}</div>
                                                <div style="font-size: 14px;margin: 2px">{{good.desc}}</div>
                                                <div style="font-size: 14px;margin-top: 0">Monthly sale: {{good.sales}}</div>
                                                <div style="font-size: 14px;display: flex;justify-content: space-between;padding: 0px 30px;">
                                                    <span style="float: left;color: crimson;display: inline-block">${{good.price}}</span>
                                                    <!--<van-stepper style="margin:10px;display:inline-block;" :default-value="0" theme="round" button-size="15" disable-input @plus="add" @minus="reduce"/>-->
                                                    <template>
                                                        <button style="border-radius: 50%;border: solid 1px" @click="reduce(index1,index2)" v-show="good.count>0">-</button>
                                                        <input type="text" v-model="good.count"  style="border: none;width: 20px;text-align: center" disabled="true" v-show="good.count>0"/>
                                                        <button style="border-radius: 50%;border: solid 1px; left: 0" @click="add(index1,index2)" >+</button>
                                                    </template>
                                                </div>
                                                <!-- <van-divider /> -->
                                            </div>
                                        </div>

                                    </li>
                                </ul>
                            </el-tab-pane>

                        </template>
                    </el-tabs>
                   <!-- <template v-for="item in goodList.category" >
                        <el-tab-pane :label="item.name">
                            <ul v-for="good in item.goods">
                                <li>{{good.name}}</li>
                            </ul>
                        </el-tab-pane>
                    </template>-->
                    <!--<el-tab-pane label="" lazy></el-tab-pane>
                    <el-tab-pane label=""></el-tab-pane>
                    <el-tab-pane label=""></el-tab-pane>
                    <el-tab-pane label=""></el-tab-pane>-->
                </van-tab>

                <!-- <van-tab title="" name="comment">


                </van-tab>

                <van-tab title="" name="sell">


                </van-tab> -->

            </van-tabs>
            <!---->
            <div style="height:50px;display:block;"></div>
        </div>

        <!---->
        <div class="shoppingCartN" style="width: 60%" v-if="shoppingCart">
           <div class="circleN" style="">
               <!---->
               <div class="carN" style="" v-if="totalCount===null" @click="showSelectedGoods">
                   <van-icon name="shopping-cart" size="30" color="#80858a" :badge="totalCount"/>
               </div>
               <!---->
               <div class="carN" style="background-color: #00a0dc" v-if="totalCount > 0" @click="showSelectedGoods">
                   <van-icon name="shopping-cart" size="30" color="white" :badge="totalCount"/>
               </div>
           </div>
            <div style="border-right: 1px solid #51575f;left:55px;width: 80px;height: 30px;display: inline-block;position: absolute;margin: 10px 5px;color: #70757b" v-if="totalPrice===0">
                ${{goodTotalPrice}}
            </div>
            <div style="border-right: 1px solid #51575f;left:55px;width: 80px;height: 30px;display: inline-block;position: absolute;margin: 10px 5px;color: #fdfdfd" v-if="totalPrice>0">
                ${{goodTotalPrice}}
            </div>
            <!-- <div style="left:150px;color: #70757b;font-size: 12px;display:inline-block;position: absolute;margin:12px 0 0 5px;height: 44px">
                ${{deliveryPrice}}
            </div> -->
            <div style="float: right;width: 80px;">
                <van-button round size="large" style="background-color: #70757b;border-color: #70757b;font-size: 12px;margin-top: 5px;height: 40px" v-if="this.goodTotalPrice===0">Please place an order</van-button>
                <!-- <van-button round size="large" style="color: white;background-color: #70757b;border-color: #70757b;font-size: 12px;margin-top: 5px;height: 40px" v-if="this.goodTotalPrice < this.minPrice"></van-button> -->
                <van-button round type="info" size="large" style="font-size: 12px;margin-top: 5px;height: 40px" v-if="this.goodTotalPrice > 0" @click="toPay">Go to settlement</van-button>
            </div>
        </div>

        <!---->
        <el-drawer class="User_drawer"
                :visible.sync="isShowSelectedGoods"
                direction="btt"
                :show-close="false"
                :with-header="false"
                :size="400">
                <div class="User_drawer_list">
                    <div style="text-align: left">
                        <van-tag color="#ffe1e1" text-color="#ad0000" ><span style="margin:10px;"> <van-icon name="cart" />Selected items</span></van-tag>
                    </div>
                    <div>
                <template>
                        <ul >
                            <li v-for="(good,index) in selectedGoods" :key="index">
                                <div style="border-radius: 10px;background-color: white">
                                    <div style="display:inline-block;float: left">
                                        <img :src="Phototimg(good.goodPic)" width="60" height="60" alt="">
                                    </div>
                                    <div>
                                        <h4 style="width: 160px;margin-bottom: 2px;margin-top: 10px">{{good.goodName}}</h4>
                                        <div style="width: 100%;font-size: 15px;margin-top: 20px;height: 20px">
                                            <span style="float: left;color: crimson;display: inline-block;margin-left: 20px">${{good.price*good.count}}</span>
                                            <div style="float: right;margin-right:10px">
                                                <button style="border-radius: 50%;border: solid 1px" @click="reduceCar(index)" v-show="good.count>0">-</button>
                                                <input type="text" v-model="good.count"  style="border: none;width: 20px;text-align: center" disabled="true" v-show="good.count>0"/>
                                                <button style="border-radius: 50%;border: solid 1px; left: 0" @click="addCar(index)" >+</button>
                                            </div>
                                        </div>
                                        <van-divider />
                                    </div>
                                </div>

                            </li>
                        </ul>

                </template>
            <!---->
            <div style="height:50px;display:block;"></div>
        </div>
                </div>
            
        
        <div>

        </div>
        </el-drawer>


        <!---->
        <div style="width:100%; height: 50px;bottom: 0; position: fixed;background-color: #ffffff; z-index: 100" v-if="hider">

        </div>

    </div>

        <!---->
        <div style="width: 60%;background-color:white;z-index: 99999;float:left;margin-left: 20%;" v-if="showPay">
            <van-nav-bar
                    title="Confirm order"
                    left-text="Back"
                    left-arrow
                    @click-left="isShowPay"
            />
            <van-notice-bar
                    left-icon="volume-o"
                    text="Save food, start from me, start from now on. Please order in moderation to avoid waste"
            />
            <!-- <div style="width: 300px;;margin: 10px;border-radius: 10px;border: solid 1px">
                <div style="width: 300px;margin-bottom: 5px;">
                    <span v-if="addressMessage===null"><h3 style="display:inline-block;margin-bottom:0;margin-top: 5px;margin-left: 5px;margin-right: 10px"></h3></span>
                    <span v-if="addressMessage!==null">
                        <div style="font-size: 13px;width: 150px;display: inline-block;">
                            <div>
                                {{addressMessage.address}}
                            </div>
                            <div>
                                <span>
                                    {{addressMessage.name}}
                                </span>
                                    {{addressMessage.tel}}
                                <span>
                            </span>
                            </div>
                        </div>

                    </span>
                    <span style="margin-left: 120px;"><van-icon name="arrow" @click="chooseAddress"/></span>
                </div>
                <div style="width: 300px;height: 40px;margin: 0;">
                    <span><h4 style="display:inline-block;margin-bottom:0;margin-top: 5px;margin-left: 5px;margin-right: 50px"></h4></span>
                    <span style="margin-left: 50px;"><van-icon name="clock-o" />12:50 <van-icon name="arrow" @click="chooseTime"/></span>

                </div>
                <div style="width: 300px;height: 40px;margin: 0;">
                    <span><h4 style="display:inline-block;margin-bottom:0;margin-top: 5px;margin-left: 5px;margin-right: 50px"></h4></span>
                    <span style="margin-left: 100px;"><van-icon name="arrow" @click="choosePayType"/></span>

                </div>

            </div> -->

            <div style="margin: 10px;border-radius: 10px;padding: 5px;">
                <template>
                    <ul>
                        <li v-for="(good,index) in selectedGoods" :key="index">
                            <div style="border-radius: 10px;background-color: white">
                                <div style="display:inline-block;float: left">
                                    <img :src="Phototimg(good.goodPic)" width="60" height="60" alt="">
                                </div>
                                <div>
                                    <h4 style="width: 160px;margin-bottom: 2px;margin-top: 10px">{{good.goodName}}</h4>
                                    <div style="width: 100%;font-size: 15px;margin-top: 20px;height: 20px">
                                        <span style="float: left;color: crimson;display: inline-block;margin-left: 20px">×{{good.count}}</span>
                                        <div style="float: right;margin-right:10px">
                                            <span style="float: left;color: crimson;display: inline-block;margin-left: 20px">${{good.price*good.count}}</span>
                                        </div>
                                    </div>
                                    <van-divider />
                                </div>
                            </div>
                        </li>
                    </ul>
                    <!-- <div>
                        <h4 style="width: 50px;margin-left: 10px;display: inline-block;margin-right: 180px;margin-top: 5px;margin-bottom: 5px"></h4>
                        <span>${{deliveryPrice}}</span>
                    </div> -->
                    <div style="text-align: start;">
                        <div style="font-size: 18px;font-weight: bold;margin: 20px 0;">Select payment method</div>
                        <el-radio-group v-model="radio">
                            <div class="User_group_style">
                                <el-radio :label="3">
                                    <img style="width: 150px;height: 50px;" src="../../assets/bank1.png" />
                                </el-radio>
                            </div>
                            <div class="User_group_style">
                                <el-radio :label="6">
                                    <img style="width: 150px;height: 50px;" src="../../assets/bank2.png" />
                                </el-radio>
                            </div>
                            <div class="User_group_style">
                                <el-radio :label="9">
                                    <img style="width: 150px;height: 50px;" src="../../assets/bank3.png" />
                                </el-radio>
                            </div>
                        </el-radio-group>
                    </div>
                </template>
            </div>
            <!---->
            <div style="height:50px;display:block;"></div>

            <!--+-->
            <div style="height: 50px;bottom: 0; position: fixed;background-color: #409EFF; z-index: 100">
                <van-submit-bar style="width: 60%;margin-left: 20%;" :price="(goodTotalPrice*100)" button-text="Immediate payment" @submit="submit" button-type="info"/>
            </div>
        </div>

        <!---->
        <div v-if="showAddress">
            <div>
                <slot><van-nav-bar
                        title=""
                        left-text=""
                        left-arrow
                        @click-left="backPay"
                /></slot>
                <van-address-list
                        v-model="chosenAddressId"
                        :list="addressList"
                        @add="onAdd"
                        @select="onSelect"
                />
            </div>
        </div>

        <!---->
        <div v-if="showAddAddress">
            <slot><van-nav-bar
                    title=""
                    left-text=""
                    left-arrow
                    @click-left="backChooseAddress"
            /></slot>
            <van-address-edit
                    :area-list="areaList"
                    show-set-default
                    :area-columns-placeholder="['', '', '']"
                    @save="onSave"
            />
            <!---->
            <div style="width:100%; height: 50px;bottom: 0; position: fixed;background-color: #ffffff; z-index: 100" v-if="showAddAddress">

            </div>
            <!--:search-result="searchResult"
            :search-result="searchResult"-->
        </div>


        <!---->
       <!-- <div v-if="showPayMoney">
            <slot><van-nav-bar
                    title=""
                    left-text=""
                    left-arrow
                    @click-left="backPay"
            /></slot>
            &lt;!&ndash;&ndash;&gt;
            <div style="width:100%; height: 50px;bottom: 0; position: fixed;background-color: #ffffff; z-index: 100" v-if="showPayMoney">

            </div>
        </div>-->
    </div>

</template>
<style lang="scss" scoped>
.User_group_style {
    .el-radio  {
        display: flex;
        align-items: center;
    }
}
.User_drawer {
      width: 60%;
      margin-left: 20%;
      z-index: 10;
      .el-drawer__body {
          padding: 10px !important;
      }
      }
.User_drawer_list {
    padding: 10px;
}
    /**/
    .shoppingCartN{
        width:100%;
        height: 50px;
        bottom: 0;
        position: fixed;
        background-color:#141d27;
        z-index: 9999;
    }
    .circleN{
        margin-left:10px; width: 60px;height: 60px; background-color: #141d27;text-align: center;border-radius: 50%;position: absolute;bottom: 0
    }
    .carN{
        margin-top: 10px;background-color: #2b343c;border-radius: 50%;width: 40px;height: 40px;margin-left: 10px
    }


    .minPriceDesc {
        font-size: 12px;
        font-weight: 700;
        line-height: 48px;
    }
    .not-enough {
        background: #2b333b;
    }
    .enough{
        background:#00b43c;
        color:#fff
    }

</style>
<script>

    import Toast from "vant/lib/toast";
    import { Dialog } from 'vant';
    import { areaList } from '@vant/area-data';

    export default {
        name: "Shop",

        created(){
          console.log(""+this.$route.params.shop.sid);
          this.axios.get("http://localhost:8084/findAllGoodsBySid",{
              params:{
                  sid: this.$route.params.shop.sid
              }
          }).then(resp=>{
              //console.log(resp.data)
              this.goodList = resp.data;
          });
          this.axios.get("http://localhost:8084/findShopBySid",{
              params:{
                  sid: this.$route.params.shop.sid
              }
          }).then(resp=>{
              this.shopMessage = resp.data;
              this.minPrice = resp.data.minCost;
              this.deliveryPrice = resp.data.deliveryCost;
              console.log("："+JSON.stringify(resp.data))
          })
        },
        computed:{
            onChange() {},
            /**/
            goodTotalPrice(){
                let goodTotalPrice = 0;
                this.goodList.forEach(function (item) {
                    item.goods.forEach(function (good) {
                        goodTotalPrice+= good.count*good.price;
                    })
                });
                return goodTotalPrice;
            },

            /**/
            selectedGoods() {
                //good
                let goods = [];
                this.goodList.forEach(function (item) {
                    item.goods.forEach(function (good) {
                        if(good.count>0){
                            goods.push(good);
                        }
                    })
                });
                return goods;
            },


        },
        data(){
            return{
                /**/
                /*uid:this.uid,
                    sid:this.shopId,
                    totalPrice:this.totalPrice,
                    goods:this.selectedGoods,
                    addressMessage:this.addressMessage,
                    deliveryPrice:this.deliveryPrice,*/
                orderInfo:{
                    uid:'',
                    sid:'',
                    goodTotalPrice:'',
                    deliveryPrice: '',
                    totalPrice:'',
                    goods:'',
                    addressMessage:'',
                    shopName:''
                },
                radio: 3,
                /**/
                showShopInfo:true,
                showPayMoney:false,

                /**/
                addressMessage: {
                    address: '1230',
                    name: '11111',
                    tel: '15212141214'
                },

                /**/
                showChooseAddress:true,
                areaList,
                showAddAddress:false,
                showAddress:false,
                showPay:false,

                /*address*/
                addressInfo:{
                    /*(aId=0, areaCode=null, name=null, tel=null, addressDetail=null, province=null, city=null, county=null, uid=1)*/
                    uid: this.$store.getters.getUser.id,
                    areaCode:"",
                    name:"",
                    tel:"",
                    addressDetail:"",
                    province:"",
                    city:"",
                    county:""
                },

                /**/
                totalCount:null,

                /**/
                minPrice:0,

                /*：+*/
                totalPrice:0,

                /**/
                deliveryPrice:0,

                shoppingCart:true,
                count:0,
                hider:false,
                activeKey:"a",
                activeName:'good',
                shopId:this.$route.params.shop.sid,
                //id
                uid:this.$store.getters.getUser.id,
                isShowSelectedGoods:false,
                chosenAddressId: 0,
                createTime:'',
                /**/
                shopMessage:'',

                addressList: [
                    {
                        id: '1',
                        name: '',
                        tel: '13000000000',
                        areaCode:'130105',
                        address: ' 138  7  501 ',
                        isDefault: true,
                    },
                    {
                        id: '2',
                        name: '',
                        tel: '1310000000',
                        address: ' 50 ',
                    },
                ],

                /**/
                goodList: [
                        {
                            categoryName:"",
                            goods:[
                                {
                                    gid:'',
                                    goodName:"",
                                    desc:'',
                                    goodPic:'pic/dinner.png',
                                    price:'1',
                                    sales:'100',
                                    cid:'',
                                    sid:'',
                                    count:0
                                }
                            ]
                        }
                    ]
                }
        },

        methods:{
            // 
            Phototimg(src) {
            const a = 'http://localhost:8084/' + src
            console.log(a)
            return a
            },
            /*,*/
            submit(){
                // if(this.addressMessage === null){
                //     Toast("");
                //     return;
                // }
                const that = this;
                const orderInfo = this.orderInfo;
                this.orderInfo.uid = that.uid;
                this.orderInfo.sid = that.shopId;
                this.orderInfo.goodTotalPrice = that.goodTotalPrice;
                this.orderInfo.deliveryPrice = that.deliveryPrice;
                this.orderInfo.totalPrice = (that.deliveryPrice*1.0+that.goodTotalPrice*1.0);
                this.orderInfo.addressMessage = that.addressMessage;
                this.orderInfo.shopName = that.shopMessage.shopName;
                this.orderInfo.goods = that.selectedGoods;
                console.log(this.orderInfo);

                //
                Dialog.confirm({
                    title: 'Confirm to use this payment method',
                    message: 'Confirm payment?',
                }).then(() => {
                        // on confirm
                        //Toast("");
                        const toast = Toast.loading({
                            duration: 2000,
                            forbidClick: true,
                            message: "Payment in progress..."
                        });
                        setTimeout(()=>{
                            /**/
                            this.axios.post("http://localhost:8084/createOrder",this.orderInfo)
                                .then(resp=>{
                                    if(resp.data !== null){
                                        console.log(resp.data.orderNumber);
                                        this.$router.push({
                                                name: "paySuccess",
                                                params:{
                                                    oid:resp.data.oid,
                                                    orderNumber:resp.data.orderNumber,
                                                    goodTotalPrice:resp.data.goodTotalPrice,
                                                    deliveryPrice:resp.data.deliveryPrice,
                                                    totalPrice:resp.data.totalPrice,
                                                    goods:this.selectedGoods,
                                                    addressMessage:this.addressMessage,
                                                    shopName:this.shopMessage.shopName,
                                                    orderStat:resp.data.orderStat,
                                                    createTime:resp.data.createTime
                                                }
                                            });
                                    }else {
                                        Toast("Failed to create order due to network or other reasons")
                                    }
                        })
                        },2000);
                });

            },

            /**/
            onSelect(item){
                console.log(item);
                this.addressMessage = item;
                console.log(this.addressMessage);
                this.showAddress = false;
                this.showPay = true;
            },

            /**/
            onSave(item){
                /*let addressInfo = JSON.stringify(item);
                console.log(addressInfo);
                Toast(addressInfo);*/
                this.addressInfo.areaCode = item.areaCode;
                this.addressInfo.name = item.name;
                if(this.addressInfo.province===this.addressInfo.city){
                    this.addressInfo.addressDetail = item.province+""+item.county+""+item.addressDetail;
                }else {
                    this.addressInfo.addressDetail = item.province+""+item.city+""+item.county+""+item.addressDetail;
                }
                this.addressInfo.tel = item.tel;
                this.addressInfo.province = item.province;
                this.addressInfo.city = item.city;
                this.addressInfo.county = item.county;
                console.log(this.addressInfo.addressDetail);
                //console.log(this.addressInfo);
                this.axios.post("http://localhost:8084/addAddress",
                    this.addressInfo
                ).then(resp=>{
                    console.log("sucess");
                    Toast("Added successfully!!!");
                    setTimeout(()=>{
                        this.showAddAddress = false;
                        this.chooseAddress();
                    },2000);

                })
            },

            /**/
            onAdd(){
                this.showAddAddress = true;
                this.showAddress = false;
            },

            /**/
            backChooseAddress(){
              this.showAddress = true;
              this.showAddAddress = false;
            },

            /**/
            backPay(){
                this.showPayMoney = false;
                this.showAddress = false;
                this.showPay = true;
            },

            /*，*/
            chooseAddress(){
              //Toast("");
              const that = this;
              this.axios.get("http://localhost:8084/listAddress",{
                  params:{
                      uid:this.uid
                  }
              }).then(resp=>{
                      console.log(resp.data);
                      this.addressList = resp.data;
                      this.showPay = false;
                      this.showAddress = true;
              });

            },

            /**/
            chooseTime(){
                Toast("")
            },

            /**/
            choosePayType(){
                Toast("")
            },

            /**/
            isShowPay(){
              this.showPay=false;
              this.showShopInfo = true;
            },

            /**/
            showSelectedGoods(){
                this.isShowSelectedGoods = true;
            },

            /**/
            addCar(index){
                this.selectedGoods[index].count++;
                this.totalCount++;
            },


            reduceCar(index){
                this.selectedGoods[index].count--;
                this.totalCount--;
            },


            /*Go to settlement*/
            toPay(){
                console.log("");
                this.showPay=true;
                this.showShopInfo = false;

                /*this.goodList.category.forEach(function (item) {
                    item.goods.forEach(function (good) {
                        if(good.count>0){
                            console.log(good)
                        }
                    })
                });*/
                /*console.log(this.selectedGoods);*/
                //this.$router.push({path:'/pay'});
            },


            /**/
            hide(index){
              if(index==='good'){
                  this.shoppingCart = true;
                  this.hider = false;
                  console.log("")
              }else if(index==='comment'){
                  this.shoppingCart = false;
                  this.hider = true;
                  console.log("")
              }else if(index==='sell'){
                  this.shoppingCart = false;
                  this.hider = true;
                  console.log("")
              }
            },


            /**/
            add(index1,index2){
                let totalCount = null;
                const good = this.goodList[index1].goods[index2];
                good.count++;
                this.totalCount ++;
                //Toast(this.goodList.category[index1].goods[index2].count);
            },

            /**/
            reduce(index1,index2){
                const good = this.goodList[index1].goods[index2];
                good.count--;
                this.totalCount--;
                if(this.totalCount===0){
                    this.totalCount = null;
                }
                //Toast(this.goodList.category[index1].goods[index2].count);

            },

            /**/
            back(){
                //this.$router.push('../views/user/Search.vue')
                //this.$router.go(-1);
                //this.$router.replace({name:'search'})
                history.go(-1)
            },

            showItem(){
              console.log(this.goodList[0])
            },

            searchGoods(){
              console.log("")
            },
            /**/
            payDesc() {
                // if (this.totalPrice === 0) {
                //     return `$${this.minPrice}`;
                // } else if (this.totalPrice < this.minPrice) {
                //     let differPrice = this.minPrice - this.totalPrice;
                //     return `${differPrice}`;
                // } else if (this.totalPrice >= this.minPrice) {
                //     return 'Go to settlement';
                // }
                if (this.totalPrice === 0) {
                    return `Please order`;
                } else if (this.totalPrice >= this.minPrice) {
                    return 'Go to settlement';
                }
            },

        }
    }
</script>


