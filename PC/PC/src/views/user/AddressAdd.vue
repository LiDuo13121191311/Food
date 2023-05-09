<template>
    <div>
        <slot><van-nav-bar
                title=""
                left-text=""
                left-arrow
                @click-left="onClickLeft"
        /></slot>
        <van-address-edit
                :area-list="areaList"
                show-set-default
                :area-columns-placeholder="['', '', '']"
                @save="onSave"
                @delete="onDelete"
                @change-detail="onChangeDetail"
        />
        <!---->
        <div style="width:100%; height: 50px;bottom: 0; position: fixed;background-color: #ffffff; z-index: 100">

        </div>
        <!--:search-result="searchResult"
        :search-result="searchResult"-->
    </div>

</template>

<script>
    /*Toast*/
    import Toast from 'vant/lib/toast';
    import { areaList } from '@vant/area-data';

    export default {
        name: "AddressAdd",
        created(){
          this.id
        },
        data() {
            return {
                addressMessage:this.$route.params.addressMessage,
                areaList,
                searchResult: [],
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
                }
            };
        },
        methods: {

            onClickLeft(){
                //this.$router.history.go(-1)
                this.$router.replace('/address')
                //history.go(-1)

            },

            onSave(item) {
                //Toast('save');
                //console.log(item)
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
                    console.log("sucess")
                })
                /*this.$router.push({
                    name:'address',
                    params:{
                        addressMessage:data
                    }
                })*/

            },

            onDelete() {
                Toast('delete');
            },

            onChangeDetail(val) {
                if (val) {
                    this.searchResult = [
                        {
                            name: '',
                            address: '',
                        },
                    ];
                } else {
                    this.searchResult = [];
                }
            },
        },
    }
</script>

<style scoped>

</style>
