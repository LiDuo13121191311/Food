<template>
    <div class="content">
        <el-page-header @back="goBack" content=""></el-page-header>
        <div class="All_login_style">
            <el-button style="margin-bottom: 20px;" type="primary" icon="el-icon-edit" @click="toUser"></el-button>
            <el-button style="display: none;" type="primary" icon="el-icon-share" @click=toRider></el-button>
            <el-button style="margin-bottom: 20px;margin-left: 0px;" type="primary" icon="el-icon-share" @click=toManager></el-button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Login",
        methods:{
            toUser(){
                this.$router.replace('/toUserLogin');
            },

            toRider(){
                this.$router.replace('/toRiderLogin');
            },

            toManager(){
                this.$router.replace('/toManagerLogin');
            },

            goBack(){
                this.$router.replace('/')
            }
        }
    }
</script>

<style scoped>
    .el-button{
        width: 200px;
        height: 80px;
    }

    .registerType{
        /* width: 700px;
        height: 100px;
        margin: 250px auto;
        padding:auto 60px; */
        position: absolute;
        top: 30%;
        display: flex;
        flex-direction: column;
        align-items: center;
        left: calc(50% - 100px);
    }
</style>
