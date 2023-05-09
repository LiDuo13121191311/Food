<template>
    <el-container>
        <el-header height="80px">
            <el-page-header @back="goBack" content="User registration"></el-page-header>
        </el-header>
        <el-main>
            <el-form :model="userForm" status-icon :rules="rules" ref="userForm" label-width="80px" class="demo-ruleForm UserRegister_from">
                <el-form-item label="Username" prop="username">
                    <el-input type="text" v-model="userForm.username" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Avatar" style="text-align: left">
                    <!-- <el-upload
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
                    </el-dialog> -->
                    <el-avatar @click.native="Upstart()" shape="square" :size="150" :src="dialogImageUrl"></el-avatar>
                </el-form-item>
                <el-form-item label="Password" prop="password">
                    <el-input type="password" v-model="userForm.password" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Confirm password" prop="checkPass">
                    <el-input type="password" v-model="userForm.checkPass" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Name" prop="name">
                    <el-input type="text" v-model="userForm.name" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Cell-phone number" prop="phone">
                    <el-input type="text" v-model="userForm.phone" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button @click="resetForm('userForm')">Reset</el-button>
                    <el-button type="primary" @click="submitForm(userForm)">Register</el-button>
                </el-form-item>
            </el-form>
        </el-main>
        <!--<el-footer></el-footer>-->
    </el-container>

</template>

<script>
    import Toast from "vant/lib/toast";

    export default {
        name: "UserRegister",
        data() {
            const validateUsername = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('enter one user name'));
                } else {
                    this.axios.get("http://localhost:8084/findUserByUsername",{
                        params:{
                            username:value
                        }
                    }).then(resp=>{
                        if(resp.data==='none'){
                            callback();
                        }else {
                            callback(new Error('The user name is already registered'));
                        }
                    })
                }
            };


            const validatePass = (rule, value, callback) => {
                if (value === ''||value === ' ') {
                    callback(new Error('Please input a password'));
                } else {
                    if (this.userForm.checkPass !== '') {
                        this.$refs.userForm.validateField('checkPass');
                    }
                    callback();
                }
            };

            const validatePass2 = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('Please enter the password again'));
                } else if (value !== this.userForm.password) {
                    callback(new Error('The two passwords are inconsistent!'));
                } else {
                    callback();
                }
            };

            const validateName = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('Please enter your name'));
                } else {
                    callback();
                }
            };

            const validatePhone = (rule, value, callback) => {
                if (value === ''||value === ' ') {
                    callback(new Error('Please enter your mobile number'));
                } else {
                    this.axios.get("http://localhost:8084/findUserByPhone",{
                        params:{
                            phone:value
                        }
                    }).then(resp=>{
                        if(resp.data==='none'){
                            callback();
                        }else {
                            callback(new Error('This phone number is already registered'));
                        }
                    })

                }
            };



            return {
                //
                dialogImageUrl: '',
                dialogVisible: false,
                fileList:[],
                picture:'',
                fileLimit:1,


                userForm: {
                    username:'',
                    picSrc:'',
                    password: '',
                    checkPass: '',
                    name:'',
                    phone:'',
                },
                rules: {
                    username: [
                        { validator:validateUsername,trigger: 'blur'}
                    ],
                    password: [
                        { validator: validatePass, trigger: 'blur' }
                    ],
                    checkPass: [
                        { validator: validatePass2, trigger: 'blur' }
                    ],
                    name: [
                        { validator:validateName,trigger: 'blur'}
                    ],
                    phone: [
                        { validator:validatePhone,trigger: 'blur'}
                    ],

                }
            };
        },
        methods: {
            // 
            Upstart() {
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
                this.fileimgname = resp.data.filename
                this.dialogImageUrl = ImgUrl;
                // this.user.picSrc = resp.data.message
            })
            },
            submitForm(form) {
                const that = this;
                this.$refs['userForm'].validate((valid) => {
                    if (valid) {
                        console.log(form);
                        this.$confirm("Are you sure the information is correct?",'Registration tips',{
                            confirmButtonText: 'Determine',
                            cancelButtonText: 'Cancel',
                            type:'info'
                        }).then(()=> {
                            this.axios.post("http://localhost:8084/userRegister", form)
                                .then(resp => {
                                    if (resp.data === "success") {
                                        this.$message({
                                            message: 'Registration succeeded!!!',
                                            type: 'success'
                                        });
                                        this.$router.push("/toUserLogin")
                                    } else {
                                        this.$message({
                                            message: 'Registration failed!!!',
                                            type: 'error'
                                        });
                                    }
                                })
                        })
                    }
                });
            },


            /**/
            //
            handleSuccess(res, file) {
                this.$message({
                    type: 'info',
                    message: '',
                    duration: 6000
                });
                console.log(file);
                if (file.response.code===200) {
                    this.userForm.picSrc = file.response.message; //picture
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

            resetForm(formName) {
                this.$refs[formName].resetFields();
            },
            goBack() {
                this.$router.go('-1')
            }
        }
    }
</script>

<style scoped>
    /* .el-form{
        width: 500px;
        height: 200px;
        margin: 20px auto;
    } */

    /* .el-container{
        height: 680px;
    } */
    .el-main {
        /* background-color: #E9EEF3; */
        color: #333;
        text-align: center;
        padding: 5px;
        /* line-height: 160px; */

    }

    .el-header {
        background-color: #0e95e5;
        color: #333;
        text-align: center;
    }

    /* .el-footer{
        background-color: #B3C0D1;
        color: #333;
    } */

</style>
