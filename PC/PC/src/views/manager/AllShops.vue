<template>
    <div style="text-align: center;margin: auto;">
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
            <el-table-column
                    label=""
                    prop="shopInfo.deliveryTime" >
                <template slot-scope="scope">
                    <span v-if="scope.row.shopInfo.deliveryTime!==null">{{scope.row.shopInfo.deliveryTime}}</span>
                    <span v-if="scope.row.shopInfo.deliveryTime===null"></span>
                </template>
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.totalSales" >
            </el-table-column>
            <el-table-column
                    label=""
                    prop="userInfo.username" >
            </el-table-column>
            <el-table-column
                    label=""
                    prop="userInfo.phone" >
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.addressDetail" >
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.desc">
                <template slot-scope="scope">
                    <span v-if="scope.row.shopInfo.desc!==null">{{scope.row.shopInfo.desc}}</span>
                    <span v-if="scope.row.shopInfo.desc===null"></span>
                </template>
            </el-table-column>
            <el-table-column
                    label=""
                    prop="shopInfo.stat" >
                <template slot-scope="scope">
                    <span v-if="scope.row.shopInfo.stat===1" style="color: green"></span>
                    <span v-if="scope.row.shopInfo.stat===2" style="color: red"></span>
                </template>
            </el-table-column>
            <el-table-column
                    align="right" >
                <!--<template slot="header" slot-scope="scope">
                    <el-input
                            v-model="search"
                            size="mini"
                            placeholder=""/>
                </template>-->
                <template slot-scope="scope" style="width: 120px">
                    <el-button
                            size="mini"
                            @click="edit(scope.row)" style="width: 33px;height:50px;display: inline-block;text-align: center;padding: 2px"></el-button>
                    <el-button
                            size="mini"
                            type="danger"
                            v-if="scope.row.shopInfo.stat===1"
                            @click="changeStat(scope.row.shopInfo.sid,scope.row.shopInfo.stat)" style="margin-left: 2px;;width: 33px;height:50px;display: inline-block;padding: 2px">
                        
                    </el-button>
                    <el-button
                            size="mini"
                            type="success"
                            @click="changeStat(scope.row.shopInfo.sid,scope.row.shopInfo.stat)" style="width: 50px;display: inline-block" v-if="scope.row.shopInfo.stat===2"></el-button>
                </template>
            </el-table-column>
        </el-table>

        <!---->
        <el-dialog title="" :visible.sync="dialogVisible" style="line-height: 15px">
            <el-form :model="editShopInfo" status-icon :rules="shopRules" ref="editShopInfo" style="text-align: left">
                <el-form-item label="" :label-width="formLabelWidth" prop="shopName">
                    <el-input v-model="editShopInfo.shopName" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Logo" :label-width="formLabelWidth">
                        <el-upload
                                ref="upload"
                                action="http://localhost:8084/upload"
                                name="picture"
                                list-type="picture-card"
                                :limit="1"
                                :file-list="fileList"
                                :on-exceed="onExceed"
                                :before-upload="beforeUpload"
                                :before-remove="beforeRemove"
                                :on-preview="handlePreview"
                                :on-success="handleSuccess"
                                :on-remove="handleRemove">
                            <i class="el-icon-plus"></i>
                        </el-upload>
                        <el-dialog :visible.sync="dialogPicVisible">
                            <img width="100%" :src="dialogImageUrl" alt="">
                        </el-dialog>
                </el-form-item>
                <el-form-item label="" :label-width="formLabelWidth" prop="license">
                    <el-input v-model="editShopInfo.license" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" :label-width="formLabelWidth" prop="minCost">
                    <el-input v-model="editShopInfo.minCost" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" :label-width="formLabelWidth" prop="deliveryCost">
                    <el-input v-model="editShopInfo.deliveryCost" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="(minute)" :label-width="formLabelWidth" prop="deliveryTime">
                    <el-input v-model="editShopInfo.deliveryTime" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" :label-width="formLabelWidth" prop="totalSales">
                    <el-input v-model="editShopInfo.totalSales" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" :label-width="formLabelWidth" prop="phone">
                    <el-input v-model="editShopInfo.phone" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" :label-width="formLabelWidth" prop="addressDetail">
                    <el-input v-model="editShopInfo.addressDetail" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" :label-width="formLabelWidth" prop="desc">
                    <el-input v-model="editShopInfo.desc" autocomplete="off"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="flush"> </el-button>
                <el-button type="primary" @click="save"> </el-button>
            </span>
        </el-dialog>


        <!---->
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
    import Toast from "vant/lib/toast";

    export default {
        name: "AllShops",
        created() {
            //this.userInfo = this.$route.params.userInfo;
            this.axios.get("http://localhost:8084/findAllShopInfo",{
                params:{
                    currentPage:this.currentPage,
                    pageSize:this.pageSize
                }
            }).then(resp=>{
                console.log(resp.data);
                this.shopInfo = resp.data.shopManagerRegisterVOS;
                this.total = resp.data.count;
            })
        },
        data(){

            /**/
            const validateShopName = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };


            const validateLicense = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    if(value!==this.licenseCount){
                        this.axios.get("http://localhost:8084/findShopByLicense",{
                            params:{
                                license:value
                            }
                        }).then(resp=>{
                            if(resp.data==='none'){
                                callback();
                            }else {
                                callback(new Error(''));
                            }
                        })
                    }else {
                        callback();
                    }
                }
            };

            const validateDeliveryCost = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };

            const validateDeliveryTime = (rule, value, callback) => {
                if (value === ''||value === null) {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };

            const validateMinCost = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };

            const validateShopPhone = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };

            const validateAddressDetail = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };

            const validateDesc = (rule, value, callback) => {
                if (value === ''||value === null) {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };

            return{
                dialogPicVisible:false,

                //
                dialogImageUrl: '',
                dialogVisible: false,
                fileList:[],
                picture:'',
                fileLimit:1,

                formLabelWidth: '120px',
                form:'',
                userInfo:'',
                search:'',

                licenseCount:'',
                shopInfo:[
                    {
                        shopInfo:{
                            shopName:'123'
                        },
                        shopManagerInfo:''
                    }
                ],
                editShopInfo:{
                    shopName:'',
                    license:'',
                    deliveryCost:'',
                    deliveryTime:'',
                    minCost:'',
                    totalSales:'',
                    phone:'',
                    addressDetail:'',
                    desc:''
                },

                shopRules:{
                    shopName:[
                        { validator:validateShopName,trigger: 'blur'}
                    ],
                    license: [
                        { validator:validateLicense,trigger: 'blur'}
                    ],
                    deliveryCost:[
                        { validator:validateDeliveryCost,trigger: 'blur'}
                    ],
                    deliveryTime:[
                        { validator:validateDeliveryTime,trigger: 'blur'}
                    ],
                    minCost:[
                        { validator:validateMinCost,trigger: 'blur'}
                    ],
                    phone:[
                        { validator:validateShopPhone,trigger: 'blur'}
                    ],
                    addressDetail:[
                        { validator:validateAddressDetail,trigger: 'blur'}
                    ],
                    desc:[
                        { validator:validateDesc,trigger: 'blur'}
                    ]
                },

                pageSize:5,
                total: 0,
                currentPage: 1,
                pageSizes:[1,2,3,4,5,6,7,8,9,10],
            }
        },
        methods:{
            /**/
            changeStat(sid,stat){
                console.log("sid:"+sid+",stat:"+stat);
                console.log(stat===1);
                if(stat === 1){
                    this.$confirm("？",'',{
                        confirmButtonText: '',
                        cancelButtonText: '',
                        type:'info'
                    }).then(()=>{
                            this.axios.get("http://localhost:8084/changeShopStat",{
                                params:{
                                    sid:sid,
                                    stat:'2'
                                }
                            }).then(resp=>{
                                if(resp.data === "success"){
                                    this.$message({
                                        message:"！！！",
                                        type:'success'
                                    });
                                    this.flush();
                                }
                            })
                    })
                }else if(stat === 2){
                        this.$confirm("？",'',{
                            confirmButtonText: '',
                            cancelButtonText: '',
                            type:'info'
                        }).then(()=>{
                                this.axios.get("http://localhost:8084/changeShopStat",{
                                    params:{
                                        sid:sid,
                                        stat:'1'
                                    }
                                }).then(resp=>{
                                    if(resp.data === "success"){
                                        this.$message({
                                            message:"！！！",
                                            type:'success'
                                        });
                                        this.flush();
                                    }
                                })
                        })
                    }
                },

            //
            handleSuccess(res, file) {
                this.$message({
                    type: 'info',
                    message: '',
                    duration: 6000
                });
                console.log(file);
                if (file.response.code===200) {
                    this.shopForm.logoSrc = file.response.message; //picture
                }
            },

            //
            handleRemove(file, fileList) {
                this.$message({
                    type: 'info',
                    message: '',
                    duration: 6000
                });
            },
            //
            handlePreview(file) {
                this.dialogImageUrl = file.url;
                this.dialogVisible = true;
            },


            //
            onExceed(files, fileList) {
                this.$message({
                    type: 'info',
                    message: '',
                    duration: 6000
                });
            },

            //
            //，false，Primaryreject，
            beforeUpload(file){
                const isJPG = file.type === 'image/jpeg';
                const isGIF = file.type === 'image/gif';
                const isPNG = file.type === 'image/png';
                const isBMP = file.type === 'image/bmp';
                const isLt2M = file.size / 1024 / 1024 < 2;

                if (!isJPG && !isGIF && !isPNG && !isBMP) {
                    this.$message.error('JPG/GIF/PNG/BMP !');
                }
                if (!isLt2M) {
                    this.$message.error(' 2MB!');
                }
            },

            //
            beforeRemove(file) {
                return this.$confirm(` ${ file.name }？`);
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
            save() {
                console.log(this.editShopInfo);
                this.$refs['editShopInfo'].validate((valid) => {
                    if (valid) {
                        this.$confirm("？", '', {
                            confirmButtonText: '',
                            cancelButtonText: '',
                            type: 'info'
                        }).then(() => {
                            this.axios.post("http://localhost:8084/updateShopMessage", this.editShopInfo).then(resp => {
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
                        })
                    } else {
                        return false;
                    }
                })
            },

           /**/
            edit(item){
                console.log(item);
                this.licenseCount = item.shopInfo.license;
                this.dialogVisible = true;
                this.editShopInfo = item.shopInfo;
            },

            /**/
            handleSizeChange(val) {
                this.pageSize = val;
                const that = this;
                this.axios.get("http://localhost:8084/findAllShopInfo",{
                    params:{
                        currentPage:this.currentPage,
                        pageSize:this.pageSize
                    }
                }).then(resp=>{
                    console.log(resp.data);
                    this.shopInfo = resp.data.shopManagerRegisterVOS;
                    this.total = resp.data.count;
                })
            },

            /**/
            handleCurrentChange(val) {
                this.currentPage = val;
                const that = this;
                this.axios.get("http://localhost:8084/findAllShopInfo",{
                    params:{
                        currentPage:this.currentPage,
                        pageSize:this.pageSize
                    }
                }).then(resp=>{
                    console.log(resp.data);
                    this.shopInfo = resp.data.shopManagerRegisterVOS;
                    this.total = resp.data.count;
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
    .el-table-column{
        width: 50px;
    }
    .cell{
        width: 50px;
    }
</style>
