<template>
    <div>
        <slot><van-nav-bar
                title=""
                left-text=""
                left-arrow
                @click-left="onClickLeft"
        /></slot>
        <div v-if="list.length === 0">
            <van-empty description="，~~" />
        </div>
        <van-address-list
                v-model="chosenAddressId"
                :list="list"
                :disabled-list="disabledList"
                disabled-text=""
                default-tag-text=""
                @add="onAdd"
                @edit="onEdit"
        />
    </div>

</template>

<script>
    /*Toast*/
    import Toast from 'vant/lib/toast';

    export default {
        name: "Address",
        created(){
            console.log(this.$store.getters.getUser.id);
            this.axios.get("http://localhost:8084/listAddress",{
                params:{
                    uid: this.$store.getters.getUser.id
                }
            }).then(resp=>{
                console.log(resp.data);
                this.list = resp.data
            })
        },
        data() {
            return {
                chosenAddressId: 0,
                list: [
                    /*{
                        id: '1',
                        name: '',
                        tel: '13000000000',
                        areaCode:'130105',
                        address: ' 138  7  501 ',
                        isDefault: true,
                    },
                    {
                        id: '2',
                        name: '',
                        tel: '1310000000',
                        address: ' 50 ',
                    },*/
                ],
                disabledList: [
                    {
                        id: '3',
                        name: '',
                        tel: '1320000000',
                        address: ' 15 ',
                    },
                ],
            };
        },
        methods: {
            onAdd() {
                //Toast('');
                this.$router.push('/addressAdd')
            },
            onEdit(item, index) {
                //Toast(':' + index);
                //console.log(item)
                let data = JSON.stringify(item);
                this.$router.push({
                    name:'addressEdit',
                    params:{
                        addressMessage:data
                    }
                })
            },
            onClickLeft(){
                this.$router.push('/my_user');
            }
        },
    }
</script>

<style scoped>

</style>
