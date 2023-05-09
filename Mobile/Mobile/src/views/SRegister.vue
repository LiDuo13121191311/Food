<template>
    <!---->
    <el-main>
        <el-form :model="shopForm" status-icon :rules="shopRules" ref="shopForm" label-width="100px">
            <el-form-item label=":" prop="shopName">
                <el-input type="text" v-model="shopForm.shopName"></el-input>
            </el-form-item>
            <el-form-item label="Logo" style="text-align: left">
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
            </el-form-item>
            <el-form-item label=":" prop="license">
                <el-input type="text" v-model="shopForm.license" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="$:" prop="deliveryCost">
                <el-input type="text" v-model="shopForm.deliveryCost" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="$:" prop="minCost">
                <el-input type="text" v-model.number="shopForm.minCost"></el-input>
            </el-form-item>
            <el-form-item label=":" prop="phone">
                <el-input type="text" v-model="shopForm.phone" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label=":" prop="addressDetail">
                <el-input type="text" v-model="shopForm.addressDetail" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button @click="resetForm('shopForm')"></el-button>
                <el-button type="primary" @click="submitForm(shopForm)"></el-button>
            </el-form-item>
        </el-form>
    </el-main>
</template>

<script>
    import Toast from "vant/lib/toast";

    export default {
        name: "SRegister",
        created(){
            this.registerInfo.userInfo = this.$route.query.userInfo;
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
                }
            };

            const validateDeliveryCost = (rule, value, callback) => {
                if (value === '') {
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



            return{
                //
                dialogImageUrl: '',
                dialogVisible: false,
                //（）
                /*fileList: [{name: '', url: ''}],*/
                fileList:[],
                picture:'',
                fileLimit:1,

                registerInfo:{
                    userInfo:null,
                    shopInfo:null
                },
                shopForm: {
                    shopName:'',
                    logoSrc: '',
                    license: '',
                    deliveryCost:'',
                    minCost:'',
                    phone:'',
                    addressDetail:''
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
                    minCost:[
                        { validator:validateMinCost,trigger: 'blur'}
                    ],
                    phone:[
                        { validator:validateShopPhone,trigger: 'blur'}
                    ],
                    addressDetail:[
                        { validator:validateAddressDetail,trigger: 'blur'}
                    ]
                }
            }
        },
        methods:{
            //
            handlePreview(file) {
                this.dialogImageUrl = file.url;
                this.dialogVisible = true;
            },

            //
            handleSuccess(res, file) {
                this.$message({
                    type: 'success',
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

            submitForm(form) {
                const that = this;
                    this.$refs['shopForm'].validate((valid) => {
                        if (valid) {
                            this.registerInfo.shopInfo = form;
                            this.$emit("registerInfo",this.registerInfo);
                            this.$emit("func",2);
                        } else {
                            Toast("");
                            return false;
                        }
                    });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            },

        }
    }
</script>

<style scoped>

</style>
