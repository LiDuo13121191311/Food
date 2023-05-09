<template>
    <el-container>
        <el-header height="40px">
            <el-page-header @back="goBack" content=""></el-page-header>
        </el-header>
        <el-main>
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
                <el-form-item label="" prop="username">
                    <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" prop="pass">
                    <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" prop="checkPass">
                    <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" prop="name">
                    <el-input type="text" v-model="ruleForm.name" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" prop="phone">
                    <el-input type="text" v-model="ruleForm.phone" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="" prop="age">
                    <el-input type="number" v-model.number="ruleForm.age"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="submitForm('ruleForm')"></el-button>
                    <el-button @click="resetForm('ruleForm')"></el-button>
                </el-form-item>
            </el-form>
        </el-main>
        <el-footer></el-footer>
    </el-container>

</template>
<style>
    .el-form{
        width: 500px;
        height: 200px;
        margin: 20px auto;
    }

    .el-container{
        height: 680px;
    }
    .el-main {
        background-color: #E9EEF3;
        color: #333;
        text-align: center;
        line-height: 160px;

    }

    .el-header {
        background-color: #409EFF;
        color: #333;
        text-align: center;
    }

    .el-footer{
        background-color: #B3C0D1;
        color: #333;
    }
</style>
<script>
    export default {
        name: "RiderRegister",
        data() {
            const validateUsername = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };


            const validatePass = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    if (this.ruleForm.checkPass !== '') {
                        this.$refs.ruleForm.validateField('checkPass');
                    }
                    callback();
                }
            };

            const validatePass2 = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else if (value !== this.ruleForm.pass) {
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

            const validatePhone = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(''));
                } else {
                    setTimeout(()=> {
                        if(value === '123'){
                            callback(new Error('！！'))
                        }else {
                            callback();
                        }
                    },1000)

                }
            };

            const checkAge = (rule, value, callback) => {
                if (!value) {
                    return callback(new Error(''));
                }
                setTimeout(() => {
                    if (!Number.isInteger(value)) {
                        callback(new Error(''));
                    } else {
                        if (value < 18) {
                            callback(new Error('18'));
                        } else {
                            callback();
                        }
                    }
                }, 500);
            };

            return {
                ruleForm: {
                    username:'',
                    pass: '',
                    checkPass: '',
                    name:'',
                    phone:'',
                    age: ''
                },
                rules: {
                    username: [
                        { validator:validateUsername,trigger: 'blur'}
                    ],
                    pass: [
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
                    age: [
                        { validator: checkAge, trigger: 'blur' }
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                const that = this;
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        if(confirm("？")){
                            const username = that.ruleForm.username;
                            const password = that.ruleForm.pass;
                            const name = that.ruleForm.name;
                            const phone = that.ruleForm.phone;
                            const age = that.ruleForm.age;
                            /*console.log(username+","+password+"")*/
                            this.$message({
                                message: '！！！',
                                type: 'success'
                            });
                            this.$router.push("/")
                        }else {
                            return false;
                        }

                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            },
            goBack() {
                // this.$router.push("/registerType")
                this.$router.go('-1')
            }
        }
    }
</script>

<style scoped>

</style>
