<template>
<div>
    <el-main >
        <el-form :model="shopForm" status-icon :rules="shopRules" ref="shopForm" label-width="150px" style="width: 500px;margin: auto;">
            <el-form-item label="Shop name:" prop="shopName">
                <el-input type="text" :disabled="editDisabled" v-model="shopForm.shopName"></el-input>
            </el-form-item>
            <!-- <el-form-item label="Store logo" style="text-align: left">
                <img width="200px;" height="200px" :src="shopForm.logoSrc" alt="" v-if="editDisabled">
                <template v-if="!editDisabled">
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
                    <el-dialog :visible.sync="dialogVisible">
                        <img width="100%" :src="dialogImageUrl" alt="">
                    </el-dialog>
                </template>
            </el-form-item> -->
            <!-- <el-form-item label="Store logo:">
                <img width="200px;" height="200px" :src="Phototimg(shopForm.logoSrc)" alt="" v-if="editDisabled">
                <el-avatar class="Logo_logo" v-else @click.native="Upstart()" shape="square" :src="dialogImageUrl !=='' ? dialogImageUrl : Phototimg(shopForm.logoSrc)"></el-avatar>
            </el-form-item> -->
            <el-form-item label="Business license:" prop="license">
                <el-input type="text" :disabled="editDisabled" v-model="shopForm.license" autocomplete="off"></el-input>
            </el-form-item>
            <!-- <el-form-item label="$:" prop="deliveryCost">
                <el-input type="text" :disabled="editDisabled" v-model="shopForm.deliveryCost" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="(minute):" prop="deliveryTime">
                <el-input type="text" :disabled="editDisabled" v-model="shopForm.deliveryTime" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="$:" prop="minCost">
                <el-input type="text" :disabled="editDisabled" v-model.number="shopForm.minCost"></el-input>
            </el-form-item> -->
            <el-form-item label="Contact number:" prop="phone">
                <el-input type="text" :disabled="editDisabled" v-model="shopForm.phone" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="Shop detailed address:" prop="addressDetail">
                <el-input type="text" :disabled="editDisabled" v-model="shopForm.addressDetail" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="Shop description:" prop="desc">
                <el-input type="text" autosize :disabled="editDisabled" v-model="shopForm.desc" autocomplete="off" maxlength="12"
                          show-word-limit></el-input>
            </el-form-item>
            <el-form-item v-if="editDisabled">
                <el-button type="primary" @click="toEditShopForm">Modify store information</el-button>
            </el-form-item>
            <template v-if="!editDisabled">
                <el-form-item>
                    <el-button type="primary" @click="save">Save</el-button>
                    <el-button type="primary" @click="flush">Cancel</el-button>
                </el-form-item>
            </template>

        </el-form>
    </el-main>
    <!--<el-form :label-position="top" label-width="80px" :model="shopMessage">
        <el-form-item label="">
            <el-input v-model="shopMessage.shopName"></el-input>
        </el-form-item>
        <el-form-item label="">
            <el-input v-model="shopMessage.logoSrc"></el-input>
        </el-form-item>

        <el-form-item label="">
            <el-input v-model="shopMessage.license"></el-input>
        </el-form-item>
    </el-form>-->
</div>
</template>

<script>
    import Toast from "vant/lib/toast";

    export default {
        name: "ShopMessage",
        created() {
            const userInfo = JSON.parse(localStorage.getItem('shopManagerInfo'))
            const uid = userInfo.smid;
            console.log(uid);
            this.axios.get("http://localhost:8084/findShopByUid",{
                params:{
                    uid:uid
                }
            }).then(resp=>{
                console.log(resp.data);
                this.shopForm = resp.data;
                this.licenseCount = resp.data.license;
            })


        },
        data(){

            /**/
            const validateShopName = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('Please enter the store name'));
                } else {
                    callback();
                }
            };


            const validateLicense = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('Please enter the business license'));
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
                                callback(new Error('The business license has been used'));
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
                    callback(new Error('Please enter the store contact number'));
                } else {
                    callback();
                }
            };

            const validateAddressDetail = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('Please enter the detailed address of the store'));
                } else {
                    callback();
                }
            };

            const validateDesc = (rule, value, callback) => {
                if (value === ''||value === null) {
                    callback(new Error('Please enter the store description'));
                } else {
                    callback();
                }
            };

            return{
                editDisabled:true,
                fileimgname: '',

                shopForm: {
                    shopName:'',
                    logoSrc: '',
                    license: '',
                    deliveryCost:'',
                    deliveryTime:null,
                    minCost:'',
                    phone:'',
                    addressDetail:'',
                    desc:null
                },


                //
                dialogImageUrl: '',
                dialogVisible: false,
                fileList:[],
                picture:'',
                fileLimit:1,

                licenseCount:'',
                shopMessage:{
                    sid:'',
                    logoSrc:'',
                    shopName:'',
                    license:'',
                    stat:'',
                    deliveryCost:'',
                    deliveryTime:'',
                    minCost:'',
                    totalSales:'',
                    phone:'',
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
                }
            }
        },
        methods:{
            // 
            Phototimg(src) {
            const a = 'http://localhost:8084/' + src
            console.log(a)
            return a
            },
            // 
            Upstart() {
            this.dialogImageUrl = ''
            //input
            const upload = document.createElement("input");
            //typefile
            upload.type = "file";
            //
            upload.accept = "image/png, image/jpeg";
            //onchange
            upload.onchange = this.setFile;
            //
            upload.click();
            },
            setFile(e) {
            //
            const file = e.path[0].files[0];
            //formdata
            const formData = new FormData();
            formData.append("imgFile", file);
            const ImgUrl = window.URL.createObjectURL(file);
            //Image
            const img = new Image();
            img.src = ImgUrl;
            let data = new FormData()
            data.append('photo', file)
            this.axios.post("http://localhost:8084/upload_photo",data,{
                headers: { "Content-Type": "multipart/form-data" }
            }).then(resp=>{
                console.log("");
                console.log(resp.data);
                this.shopForm.logoSrc = resp.data.filename
                this.dialogImageUrl = ImgUrl;
                // this.user.picSrc = resp.data.message
            }).catch(()=> {
                this.$message.error('The picture is too large or the format is incorrect!')
            })
            },
            /**/
            toEditShopForm(){
                this.editDisabled = false;
                console.log("")
            },

            //
            save(){
                this.$refs['shopForm'].validate((valid) => {
                    if (valid) {
                        this.axios.post("http://localhost:8084/updateShopMessage",this.shopForm)
                            .then(resp=>{
                                if(resp.data === "success"){
                                    console.log("Modified successfully");
                                    this.$message({
                                        type: 'success',
                                        message: 'Modified successfully',
                                        duration: 6000
                                    });
                                    //
                                    this.flush();
                                }

                            })
                    } else {
                        Toast("Please enter store information");
                        return false;
                    }
                });
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

            //
            flush(){
                this.editDisabled = true;
                this.$router.push({
                    name:"blank",
                    params:{
                        url:"shopMessage"
                    }
                })
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>
