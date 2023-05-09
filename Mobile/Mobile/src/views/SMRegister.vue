<template>
    <!---->
    <el-main>
        <el-form :model="userForm" status-icon :rules="rules" ref="userForm" label-width="100px">
            <el-form-item label="" prop="username">
                <el-input type="text" v-model="userForm.username" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="" prop="password">
                <el-input type="password" v-model="userForm.password" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="" prop="checkPass">
                <el-input type="password" v-model="userForm.checkPass" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="" prop="name">
                <el-input type="text" v-model="userForm.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="" prop="identityNumber">
                <el-input type="text" v-model.number="userForm.identityNumber"></el-input>
            </el-form-item>
            <el-form-item label="" prop="phone">
                <el-input type="text" v-model="userForm.phone" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button @click="resetForm('userForm')"></el-button>
                <el-button type="primary" @click="submitForm(userForm)"></el-button>
            </el-form-item>
        </el-form>
    </el-main>

</template>

<script>
    import Toast from "vant/lib/toast";

    export default {
        name: "SMRegister",
        data(){
            const validateUsername = (rule, value, callback) => {
                if (value === ''||value === ' ') {
                    callback(new Error(''));
                } else {
                    this.axios.get("http://localhost:8084/findShopManagerByUsername",{
                        params:{
                            username:value
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


            const validatePass = (rule, value, callback) => {
                if (value === ''||value === ' ') {
                    callback(new Error(''));
                } else {
                    if (this.userForm.checkPass !== '') {
                        this.$refs.userForm.validateField('checkPass');
                    }
                    callback();
                }
            };

            const validatePass2 = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else if (value !== this.userForm.password) {
                    callback(new Error('!'));
                } else {
                    callback();
                }
            };

            const validateName = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };

            const validateIdentityNumber = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    this.axios.get("http://localhost:8084/findShopManagerByIdentityNumber",{
                        params:{
                            identityNumber:value
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

            const validatePhone = (rule, value, callback) => {
                if (value === ''||value === ' ') {
                    callback(new Error(''));
                } else {
                    this.axios.get("http://localhost:8084/findShopManagerByPhone",{
                        params:{
                            phone:value
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
            return{
                userForm: {
                    username:'',
                    password: '',
                    checkPass: '',
                    name:'',
                    identityNumber:'',
                    phone:'',
                },
                userInfo:{
                    username:'',
                    password: '',
                    name:'',
                    identityNumber:'',
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
                    identityNumber: [
                        { validator:validateIdentityNumber,trigger: 'blur'}
                    ],
                    phone: [
                        { validator:validatePhone,trigger: 'blur'}
                    ]
                },
            }
        },
        methods:{
            submitForm(form) {
                const that = this;
                this.userInfo.username = this.userForm.username;
                this.userInfo.password = this.userForm.password;
                this.userInfo.name = this.userForm.name;
                this.userInfo.phone = this.userForm.phone;
                this.userInfo.identityNumber = this.userForm.identityNumber;
                    this.$refs['userForm'].validate((valid) => {
                        if (valid) {
                            this.$emit("func",1);
                            this.$router.push({
                                path:"/shopRegister",
                                query:{
                                    userInfo:this.userInfo
                                }
                            })
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
