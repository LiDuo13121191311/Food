<template>
    <div >
        <div style="height: 150px;margin-top: 70px">
            <div style="">
                <van-image
                        round
                        width="150"
                        height="150"
                        src="https://img01.yzcdn.cn/vant/cat.jpeg"
                />
            </div>
            <div style="margin-top: 50px">
                <van-tag color="#ffe1e1" size="large" text-color="#ad0000">vvvip</van-tag>
                <span style="color: #409EFF;" v-model="deliveryMan.name">   {{deliveryMan.name}}</span>
                <h4 style="color: #409EFF;" v-model="deliveryMan.totalNum">： {{deliveryMan.totalNum==null?0:deliveryMan.totalNum}}  </h4>
            </div>
            <div style=" margin-top: 20px">
                <van-button round type="info" style="width: 200px" @click="updateMessage"></van-button>
                <br>
                <br>
                <van-button round type="info" style="width: 200px" @click="loginOut">Log out</van-button>
            </div>

        </div>

    </div>


</template>

<script>

    import Toast from "vant/lib/toast";

    export default {
        name: "My",
        created(){
            this.rider = this.$store.getters.getRider;
            this.axios.get("http://localhost:8084/findDeliveryByDid",{
                params:{
                    did:this.rider.did
                }
            }).then(resp=>{
                console.log(resp.data);
                this.deliveryMan = resp.data;
            })
        },
        data(){
            return {
                deliveryMan:{
                    name:'',
                    totalNum:'',
                }

            }
        },
        methods:{
            updateMessage(){
                /*console.log("");
                this.$router.push({
                    name:'message',
                    //，get
                    /!* query:{
                         username:this.username,
                         password:this.password
                     }*!/

                    //，post
                    params:{
                        username:this.username,
                        password:this.password
                    }
                });*/
            },

            //Log out
            loginOut(){
                Toast("Log out");
                this.$store.commit("SET_RIDERINFO",null);
                localStorage.removeItem("shopdata");
                localStorage.removeItem("Admindata");
                localStorage.removeItem("managerInfo");
                localStorage.removeItem("shopManagerInfo");
                localStorage.removeItem("sid");
                localStorage.removeItem("userInfo");
                this.$router.push("/toRiderLogin");
            }
        }
    }
</script>

<style scoped>

</style>
