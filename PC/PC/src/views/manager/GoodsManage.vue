<template>
    <div style="text-align: center;margin: auto">
        <div style="height: 50px;">
            <el-button type="primary"
                       @click="add" style="float: left">Add</el-button>
        </div>
        <el-table
                :data="goods"
        style="width:100%;border-radius: 4px">
            <el-table-column
                    label="Trade name"
                    prop="goodName" >
            </el-table-column>
            <el-table-column
                    label="Picture"
                    prop="goodPic" >
                <template slot-scope="scope">
                    <el-image :src="Phototimg(scope.row.goodPic)" style="width: 100px"/>
                </template>
            </el-table-column>
            <el-table-column
                    label="Price"
                    prop="price" >
            </el-table-column>
            <el-table-column
                    label="Category"
                    prop="category" >
            </el-table-column>
            <el-table-column
                    label="Sales"
                    prop="sales" >
            </el-table-column>
            <el-table-column
                    label="Desc"
                    prop="desc" >
            </el-table-column>
            <el-table-column label="Operation" width="160" >
                <template slot-scope="scope">
                    <el-button size="mini" @click="edit(scope.row)">Edit</el-button>
                    <el-button size="mini" type="danger" @click="del(scope.row)">Delete</el-button>
                </template>
            </el-table-column>
        </el-table>

        <!---->
        <el-dialog title="Commodity information" :visible.sync="addDialogVisible" style="line-height: 15px;height: 1200px">
            <el-form :model="newGoodInfo" status-icon :rules="newGoodRules" ref="newGoodInfo" style="text-align: left">
                <el-form-item label="Trade name" :label-width="formLabelWidth" prop="goodName">
                    <el-input v-model="newGoodInfo.goodName" autocomplete="off"  placeholder="Please enter the product name"></el-input>
                </el-form-item>
                <el-form-item label="Picture" :label-width="formLabelWidth">
                    <el-avatar @click.native="Upstart()" shape="square" :size="150" :src="dialogImageUrl"></el-avatar>
                </el-form-item>
                <el-form-item label="Price" :label-width="formLabelWidth" prop="price">
                    <el-input v-model="newGoodInfo.price" autocomplete="off" placeholder="Please enter the commodity price"></el-input>
                </el-form-item>
                <el-form-item label="Category" :label-width="formLabelWidth" prop="category">
                    <el-select v-model="newGoodInfo.cid" placeholder="Please select product category">
                            <el-option
                                v-for="item in categories"
                                :key="item.cid"
                                :label="item.categoryName"
                                :value="item.cid">
                            </el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="Desc" :label-width="formLabelWidth" prop="desc">
                    <el-input v-model="newGoodInfo.desc" autocomplete="off" placeholder="Please enter product description"  clearable maxlength="10"
                              show-word-limit></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="flush">Cancel</el-button>
                <el-button type="primary" @click="addSave">Save</el-button>
            </span>
        </el-dialog>

        <!---->
        <el-dialog title="Commodity information" :visible.sync="editDialogVisible" style="line-height: 15px">
            <el-form :model="goodInfo" status-icon :rules="goodRules" ref="goodInfo" style="text-align: left">
                <el-form-item label="Trade name" :label-width="formLabelWidth" prop="goodName">
                    <el-input v-model="goodInfo.goodName" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Picture" :label-width="formLabelWidth">
                    <el-avatar @click.native="Upstart()" shape="square" :size="150" :src="dialogImageUrl !=='' ? dialogImageUrl : Phototimg(goodInfo.goodPic)"></el-avatar>
                </el-form-item>
                <el-form-item label="Price" :label-width="formLabelWidth" prop="price">
                    <el-input v-model="goodInfo.price" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Category" :label-width="formLabelWidth" prop="category">
                    <el-select v-model="goodInfo.cid" placeholder="Please select product category">
                            <el-option
                                v-for="item in categories"
                                :key="item.cid"
                                :label="item.categoryName"
                                :value="item.cid">
                            </el-option>
                    </el-select>
                </el-form-item>
                <!--<el-form-item label="" :label-width="formLabelWidth" prop="deliveryCost">
                    <el-input v-model="sales" autocomplete="off"></el-input>
                </el-form-item>-->
                <el-form-item label="Desc" :label-width="formLabelWidth" prop="desc">
                    <el-input v-model="goodInfo.desc" autocomplete="off"  clearable></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="flush">Cancel</el-button>
                <el-button type="primary" @click="save">Save</el-button>
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
    export default {
        name: "GoodsManage",
        created(){
            const data = JSON.parse(localStorage.getItem("shopManagerInfo"))
            this.sid = JSON.parse(localStorage.getItem("sid"))
            const pageSize = this.pageSize;
            const currentPage = this.currentPage;
            this.axios.get("http://localhost:8084/findGoodsAndCategoryByPage",{
                params:{
                    // sid: this.sid,
                    uid: data.smid,
                    pageSize: pageSize,
                    currentPage: currentPage
                }
            }).then(resp=>{
                this.total = resp.data.totalCount;
                console.log(resp.data);
                this.goods = resp.data.goods;
            })
        },
        methods: {
            // 
            Phototimg(src) {
            const a = 'http://localhost:8084/' + src
            console.log(a)
            return a
            },
            /**/
            addSave(){
                this.newGoodInfo.sid = this.sid;
                // this.newGoodInfo.goodPic = this.fileimgname
                this.$refs['newGoodInfo'].validate((valid) => {
                    if (valid) {
                        this.axios.post("http://localhost:8084/addGoodMessage", this.newGoodInfo).then(resp => {
                            if (resp.data === "success") {
                                this.$message({
                                    message: "Added successfully！！！",
                                    type: 'success'
                                });
                                this.flush();
                            } else {
                                this.$message({
                                    message: "Add failed！！！",
                                    type: 'warning'
                                });
                                this.flush();
                            }
                        })
                    }
                })
            },

            /**/
            add(){
                this.addDialogVisible = true;
                this.getAllCategories(null);
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
            this.dialogImageUrl = ''
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
            if (file.type !== 'image/jpeg') {
                this.$message({
                    type: 'warning',
                    message: 'The picture is too large or the format is incorrect'
                })
                return false
            }
            this.axios.post("http://localhost:8084/upload_photo",data,{
                headers: { "Content-Type": "multipart/form-data" }
            }).then(resp=>{
                console.log("");
                console.log(resp.data);
                this.goodInfo.goodPic = resp.data.filename
                this.newGoodInfo.goodPic = resp.data.filename
                this.dialogImageUrl = ImgUrl;
                // this.user.picSrc = resp.data.message
            })
            },

            /**/
            edit(row) {
                this.dialogImageUrl = ''
                this.editDialogVisible = true;
                console.log(row);
                this.getAllCategories(row);
            },

            del(row){
                this.$confirm('Delete this item, continue?', 'Tips', {
                    confirmButtonText: 'Determine',
                    cancelButtonText: 'Cancel',
                    type: 'warning'
                }).then(() => {
                    this.axios.get("http://localhost:8084/deleteGood", { 
                        params: {  
                            gid: row.gid
                        }
                    }).then(resp => {
                        if (resp.data === "success") {
                            this.$message({
                                message: "Deleted successfully！！！",
                                type: 'success'
                            });
                            this.flush();
                        } else {
                            this.$message({
                                message: "Delete failed！！！",
                                type: 'warning'
                            });
                            this.flush();
                        }
                    })
                })
            },

            //
            getAllCategories(row){
                if(row!==null) {
                    this.goodInfo = row;
                }else {
                    this.newGoodInfo = {};
                }
                this.axios.get("http://localhost:8084/getAllCategoryBySid",{
                    params:{
                        sid:this.sid
                    }
                }).then(resp=>{
                    console.log(resp.data);
                    this.categories = resp.data;
                })
            },

            /**/
            flush(){
                this.dialogVisible = false;
                this.$router.push({
                    name:"blank",
                    params:{
                        url:"goodsManage"
                    }
                })
            },

            /**/
            save() {
                // this.goodInfo.goodPic = this.fileimgname
                this.$refs['goodInfo'].validate((valid) => {
                    if (valid) {
                        this.axios.post("http://localhost:8084/updateGoodMessage", this.goodInfo).then(resp => {
                            if (resp.data === "success") {
                                this.$message({
                                    message: "Saved successfully！！！",
                                    type: 'success'
                                });
                                this.flush();
                            } else {
                                this.$message({
                                    message: "Save failed！！！",
                                    type: 'warning'
                                });
                                this.flush();
                            }
                        })
                    }
                })
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
                    this.goodInfo.goodPic = file.response.message; //picture
                }
            },

            handleSuccessAdd(res, file) {
                this.$message({
                    type: 'info',
                    message: '',
                    duration: 6000
                });
                console.log(file);
                if (file.response.code===200) {
                    this.newGoodInfo.goodPic = file.response.message; //picture
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
            handleSizeChange(val) {
                this.pageSize = val;
                const data = JSON.parse(localStorage.getItem("shopManagerInfo"))
                this.axios.get("http://localhost:8084/findGoodsAndCategoryByPage",{
                    params:{
                        uid:data.smid,
                        pageSize: this.pageSize,
                        currentPage: this.currentPage
                    }
                }).then(resp=>{
                    this.total = resp.data.totalCount;
                    console.log(resp.data);
                    this.goods = resp.data.goods;
                });
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                const data = JSON.parse(localStorage.getItem("shopManagerInfo"))
                this.axios.get("http://localhost:8084/findGoodsAndCategoryByPage",{
                    params:{
                        uid:data.smid,
                        pageSize: this.pageSize,
                        currentPage: this.currentPage
                    }
                }).then(resp=>{
                    this.total = resp.data.totalCount;
                    console.log(resp.data);
                    this.goods = resp.data.goods;
                });
            }
        },

        data() {

            /**/
            const validateGoodName = (rule, value, callback) => {
                if (value === ''||value==null) {
                    callback(new Error('Please enter the trade name'));
                } else {
                    if(value!==this.goodInfo.goodName){
                        this.axios.get("http://localhost:8084/checkGoodName",{
                            params:{
                                goodName:value
                            }
                        }).then(resp=>{
                            if(resp.data==='none'){
                                callback();
                            }else {
                                callback(new Error('The product name already exists!!'));
                            }
                        })
                    }else {
                        callback();
                    }
                }
            };

            const validateNewGoodName = (rule, value, callback) => {
                if (value === ''||value==null) {
                    callback(new Error('Please enter the trade name'));
                } else {
                    if(value!==this.newGoodInfo.goodName){
                        this.axios.get("http://localhost:8084/checkGoodName",{
                            params:{
                                goodName:value
                            }
                        }).then(resp=>{
                            if(resp.data==='none'){
                                callback();
                            }else {
                                callback(new Error('The product name already exists!!'));
                            }
                        })
                    }else {
                        callback();
                    }
                }
            };


            /*const validateCategoryName = (rule, value, callback) => {
                if (value === ''||value==null) {
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
            };*/

            const validatePrice = (rule, value, callback) => {
                if (value === ''||value==null) {
                    callback(new Error('Please enter price'));
                } else {
                    callback();
                }
            };

            /*const validateSales = (rule, value, callback) => {
                if (value === ''||value==null) {
                    callback(new Error(''));
                } else {
                    callback();
                }
            };*/

            const validateDesc = (rule, value, callback) => {
                if (value === ''||value === null) {
                    callback(new Error('Please enter product description'));
                } else {
                    callback();
                }
            };




            return {
                fileimgname: '',
                goodInfo:{
                    cid:'',
                    category:'',
                    goodName: '',
                    goodPic: '',
                    price: '',
                    sales: '',
                    desc: ''
                },
                newGoodInfo:{
                    cid:'',
                    sid:'',
                    category:'',
                    goodName: '',
                    goodPic: '',
                    price: '',
                    sales: '',
                    desc: ''
                },
                categories:[],
                //
                editDialogVisible: false,
                addDialogVisible: false,

                formLabelWidth: '120px',

                //
                dialogImageUrl: '',
                dialogPicVisible: false,
                fileList:[],
                picture:'',
                fileLimit:1,

                /**/
                pageSize: 5,
                total: 0,
                currentPage: 1,
                pageSizes: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],

                search:'',

                goodRules:{
                    goodName:[
                        { validator:validateGoodName,trigger: 'blur'}
                    ],
                    /*categoryName:[
                        { validator:validateCategoryName,trigger: 'blur'}
                    ],*/
                    price:[
                        { validator:validatePrice,trigger: 'blur'}
                    ],
                    /*sales:[
                        { validator:validateSales,trigger: 'blur'}
                    ],*/
                    desc:[
                        { validator:validateDesc,trigger: 'blur'}
                    ],

                },
                newGoodRules:{
                    goodName:[
                        { validator:validateNewGoodName,trigger: 'blur'}
                    ],
                    /*categoryName:[
                        { validator:validateCategoryName,trigger: 'blur'}
                    ],*/
                    price:[
                        { validator:validatePrice,trigger: 'blur'}
                    ],
                    /*sales:[
                        { validator:validateSales,trigger: 'blur'}
                    ],*/
                    desc:[
                        { validator:validateDesc,trigger: 'blur'}
                    ],

                },

                goods: []
            }
        }
    }
</script>

<style scoped>
.el-table{
    line-height: 50px;
    margin: auto;
}

    .cell{
        width: 50px;
    }
</style>
