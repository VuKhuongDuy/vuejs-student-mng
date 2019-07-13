<template>
    <div id="footer">
        Liên hệ : {{email}}
    </div>
</template>

<script>
export default {
    data: function(){
        return {
            email: 'vukhuongduy23@gmail.com'
        }
    }
}
</script>

<style scoped>
    #footer{
        width: 100%;
        height: 40px;
        color: white;
        background-color: #666666;
        text-align: center;
        font-size: 23px;
        padding-top: 7px;
    }
</style>

