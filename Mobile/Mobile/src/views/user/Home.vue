<template>
  <div id="app">
<!--    <div id="nav">-->
      <!-- <div v-show="showSearch" class="User_header_search">
          <van-search placeholder="" v-on:click="search"/>
    </div> -->
    <!--</div>-->
    <div>
      <swipe></swipe>
    </div>
    <div>
      <van-image
              width="50"
              height="50"
              src="pic/dinner.png"
              style="margin: 10px"
      />
      <van-image

              width="50"
              height="50"
              src="pic/fruit.png"
              style="margin: 10px"
      />

    </div>
          <van-tag color="#ffe1e1" size="large" text-color="#ad0000" ><van-icon name="fire-o" />hot</van-tag>
          <h4 style="display: inline-block;font-size: 20px"></h4>
      <van-card  v-for="(shop,index) in shops" :key="index" @click="toShop(shop)" style="padding-bottom: 18px;margin: 8px 10px;">
          <template #title>
              <span><h3 style="margin: 5px">{{shop.shopName}}</h3></span>
          </template>
        <template #num>
            <span>Monthly sale{{shop.totalSales}}</span>
        </template>
          <!-- <template #thumb>
              <img :src="shop.logoSrc" style="width: 100px;height: 100px" alt=""/>
          </template> -->
          <!-- <template #desc>
              <div>
                  <span>${{shop.minCost}}</span>&nbsp;
                  <span v-if="shop.deliveryCost!==0">{{shop.deliveryCost}}$</span>
                  <span v-if="shop.deliveryCost===0"></span>
              </div>
          </template> -->

        <!-- <template #tags>
          <van-tag plain type="danger">Top{{index+1}}</van-tag>
        </template> -->
      </van-card>

    <!---->
    <div style="height:50px;display:block;"></div>

  </div>
</template>

<script>
  import Swipe from "../../views/user/Swipe.vue";
  export default {
    components:{
      Swipe
    },
    methods:{
        /**/
      search(){
        this.showSearch=false;
        this.$router.replace({
            path:'/search',
        });
      },

      /**/
      toShop(shop){
        this.$router.push({
            name:"shop",
            params:{
                shop: shop
            }
        });
      },

    },

      created(){
        /*，*/
          this.axios.get("http://localhost:8084/findShopOrderBySales")
              .then(resp=>{
                  console.log(resp.data);
                  this.shops = resp.data;
          })
      },

    data() {
      return {
        value:'',
        shops:[
          
        ],
        homeShow:true,
        showSearch:true,
        show:true,
        active: 'home'
      };
    }
  };
</script>
<style>

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  #nav {
    padding: 30px;
  }

  #nav a {
    font-weight: bold;
    color: #2c3e50;
  }

  #nav a.router-link-exact-active {
    color: #42b983;
  }
</style>
<style lang="scss" scoped>
.User_header_search {
  width: 60%;
  margin-left: 20%;
}

@media screen and (max-width: 450px) {
  // ：
  .User_header_search {
    width: 100%;
    margin-left: 0;
  }
}
</style>