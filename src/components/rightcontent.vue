<template>
    <view class="boxall">
        <view class="alltitle">模块标题样式</view>
        <view class="allnav">
            <img src="../assets/vue.svg" alt="" style="width: 130px; height: 80px;">
        </view>
        
    </view>
    <view class="boxall">
        <view class="alltitle">模块标题样式</view>
        <view class="allnav">
            <img src="../assets/vue.svg" alt=""  style="width: 130px; height: 80px;">
        </view>
    </view>
    <view class="boxall">
        <view class="alltitle">模块标题样式</view>
        <view class="allnav">
            <img src="../assets/vue.svg" alt=""  style="width: 130px; height: 80px;">
        </view>
    </view>
</template>

<script  setup>



</script>

<style>
.boxall {
    border: 10px solid rgba(25, 186, 139, .50);
    padding: 0 .2rem .4rem .15rem;
    background: rgba(255, 255, 255, .20);
    background-size: 100% auto;
    position: relative;
    margin-bottom: .30rem;
    z-index: 10;
    height: 25%;
    display: flex;
    flex-direction: column;
    align-items: center
}

.alltitle {
    font-size: 1.1rem;
    color: #fff;
    text-align: center;
    line-height: .5rem;
}
.allnav{
    height: 80%;
    text-align: center;
    margin-top: 30px;

}


</style>