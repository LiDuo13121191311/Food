<template>
    <div style="text-align: center;margin: auto">
        <el-table
                :data="shopInfo"
                style="width:1450px;border-radius: 15px">
            <el-table-column
                    label=""
                    prop="shopInfo.shopName" >
            </el-table-column>
            <el-table-column
                    label="Logo"
                    prop="shopInfo.logoSrc" >
                <template slot-scope="scope">
                    <el-image :src="scope.row.shopInfo.logoSrc" style="width: 50px;height: 50px"/>
                    <!--<el-image :src="require('../../../public'+scope.row.shopInfo.logoSrc)" style="width: 50px;height: 50px"/>-->
                </template>
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.license" >
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.minCost" >
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.deliveryCost" >
            </el-table-column>
            <!--<el-table-column
                    label=""
                    prop="shopInfo.totalSales" >
            </el-table-column>-->
            <el-table-column
                    label=""
                    prop="userInfo.username" >
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.phone" >
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.addressDetail" >
            </el-table-column>
            <el-table-column
                    align="right">
                <!--<template slot="header" slot-scope="scope">
                    <el-input
                            v-model="search"
                            size="mini"
                            placeholder=""/>
                </template>-->
                <template slot-scope="scope" style="width: 110px">
                    <el-button
                            size="mini"
                            type="primary"
                            @click="agree(scope.row.shopInfo.sid)" style="width: 55px;display: inline-block;text-align: center"></el-button>
                    <!--<el-button
                            size="mini"
                            type="warning"
                            @click="edit(scope.$index, scope.row)" style="width: 55px;text-align: center"></el-button>-->
                </template>
            </el-table-column>
        </el-table>
        <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="currentPage"
                :page-sizes="pageSizes"
                :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="total">
        </el-pagination>
    </div>
</template>

<script>
    export default {
        name: "ShopApplyFor",
        created(){
          this.axios.get("http://localhost:8084/findAllShopApplyFor",{
              params:{
                  currentPage:this.currentPage,
                  pageSize:this.pageSize
              }
          }).then(resp=>{
              if(resp.data!=="none"){
                  console.log(resp.data);
                  this.shopInfo = resp.data;
                  this.total = resp.data.length;
              }
          })
        },
        data(){
            return{

                search:'',
                shopInfo:[],

                //
                pageSize:5,
                total: 0,
                currentPage: 1,
                pageSizes:[1,2,3,4,5,6,7,8,9,10],
            }
        },
        methods:{

            /**/
            agree(sid){
                console.log("sid:"+sid);
                this.$confirm("？",'',{
                    confirmButtonText: '',
                    cancelButtonText: '',
                    type:'info'
                }).then(()=>{
                    this.axios.get("http://localhost:8084/agreeShopApplyFor",{
                        params:{
                            sid:sid
                        }
                    }).then(resp=>{
                        if(resp.data==="success"){
                            this.$message({
                                message:"！！！",
                                type:'success'
                            });
                            this.flush();
                        }else {
                            this.$message({
                                message:"！！！",
                                type:'warning'
                            });
                            this.flush();
                        }
                    })
                })
            },

            /**/
            flush(){
                this.dialogVisible = false;
                this.$router.push({
                    name:"blank",
                    params:{
                        url:"allShops"
                    }
                })
            },

            /**/
            handleSizeChange(val) {
                this.pageSize = val;
                const that = this;
                this.axios.get("http://localhost:8084/findAllShopApplyFor",{
                    params:{
                        currentPage:this.currentPage,
                        pageSize:this.pageSize
                    }
                }).then(resp=>{
                    console.log(resp.data);
                    this.shopInfo = resp.data;
                    this.total = resp.data.length;
                })
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                const that = this;
                this.axios.get("http://localhost:8084/findAllShopApplyFor",{
                    params:{
                        currentPage:this.currentPage,
                        pageSize:this.pageSize
                    }
                }).then(resp=>{
                    console.log(resp.data);
                    this.shopInfo = resp.data;
                    this.total = resp.data.length;
                })
            }
        }
    }
</script>

<style scoped>
    .el-table{
        line-height: 50px;
        margin: auto;
    }
</style>
