<template>
    <div>
        <van-nav-bar
                title="Personal information"
                left-text="Back"
                left-arrow
                @click-left="onClickLeft"
        />
        <div>
            <van-form @submit="onSubmit">
                <van-field name="uploader" label="Avatar">
                    <template #input>
                        <!-- <van-uploader :max-count="1" v-model="uploader" :after-read="uploadImg" accept="image/*"/> -->
                        <el-avatar @click.native="Upstart()" shape="square" :size="150" :src="uploaderurl !== '' ? uploaderurl : Phototimg(uploader[0].url)"></el-avatar>
                    </template>
                </van-field>

                <van-field
                        v-model="user.username"
                        name="username"
                        label="Username"
                        placeholder="Username"
                        :rules="[{ required: true, message: 'Please enter Username' }]"
                />
                <van-field
                        v-model="user.password"
                        type="password"
                        name="password"
                        label="Password"
                        placeholder="Password"
                        :rules="[{ required: true, message: 'Please enter Password' }]"
                />
                <div style="margin: 16px;">
                    <van-button round block type="info" native-type="submit">Submit</van-button>
                </div>
            </van-form>
        </div>
        <!---->
        <div style="width:100%; height: 50px;bottom: 0; position: fixed;background-color: #ffffff; z-index: 100">

        </div>
    </div>
</template>

<script>
    import { Notify } from 'vant';
    export default {
        name: "Message",
        created(){
            console.log(this.$route.params.user.id);
            this.axios.get("http://localhost:8084/findUserById",{
                params:{
                    id: this.$route.params.user.id
                }
            }).then(resp=>{
                console.log("user:");
                console.log(resp.data);
                this.user = resp.data;
                let username = this.user.username;
                this.username = username;
                console.log(this.user);
                let url = {"url":this.user.picSrc};
                console.log(url);
                this.uploader.push(url);
                console.log(this.uploader);
            });

        },
        data(){
            return{
                user:'',
                uploader: [],
                username:'',
                fileimgname: '',
                uploaderurl: ''
                /*username:this.$route.params.username,
                password:this.$route.params.password*/
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
                this.uploaderurl = ''
            //
            const file = e.currentTarget.files[0];
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
                this.user.picSrc = resp.data.filename
                this.uploaderurl = ImgUrl;
                // this.user.picSrc = resp.data.message
            })
            },
            onSubmit(values){
                console.log('submit:', values);
                console.log(this.user);
                //
                if(this.username!==values.username){
                    this.axios.get("http://localhost:8084/findUserByUsername",{
                        params:{
                            username:values.username
                        }
                    }).then(resp=>{
                        if(resp.data==='exist'){
                            Notify({ type: 'warning', message: 'User name already exists!!!' });
                        }
                    })
                }
                //
                // this.user.picSrc = this.fileimgname
                this.axios.post("http://localhost:8084/updateUserMessage",this.user).then(resp=>{
                    if(resp.data === "success"){
                        Notify({ type: 'success', message: 'Modified successfully!!!' });
                    }else {
                        Notify({ type: 'warning', message: 'Modification failed!!!' });
                    }
                    this.$router.push("/my_user");
                })
            },

            onClickLeft(){
                this.$router.replace('/my_user');
            },

            /**/
            uploadImg(file){
                console.log("file:");
                console.log(file);
                let data = new FormData();
                data.append('picture',file.file);
                this.axios.post("http://localhost:8084/upload",data,{
                    headers: { "Content-Type": "multipart/form-data" }
                }).then(resp=>{
                    console.log("");
                    console.log(resp.data);
                    this.user.picSrc = resp.data.message
                })
               /* let config = {
                    headers:{'Content-Type':'multipart/form-data'} //，，Content-Type
                };
                this.axios.post("http://localhost:8084/upload", file.file,config).then(resp=>{
                    console.log("");
                    console.log(resp.data);
                })*/
            }

      }
    }
</script>

<style lang="scss" scoped>
.User_van_nav_bar {
    height: 80px;
    background: #0e95e5;
    color: #fff;
    align-items: center;
    .van-nav-bar__content {
        height: 80px;
        .van-icon {
        color: #fff !important;
        }
        .van-nav-bar__text {
            color: #fff !important;
        }
        .van-nav-bar__title {
            color: #fff !important;
        }
    }
    
}
</style>
