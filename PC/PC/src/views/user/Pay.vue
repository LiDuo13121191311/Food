<template>
    <div>
        <span @click="back"></span>
        <h1></h1>
    </div>

</template>

<script>
    export default {
        name: "Pay",
        methods:{
            back(){
                history.go(-1)
            }
        }
    }
</script>

<style scoped>

</style>
