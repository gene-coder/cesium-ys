<template>
    <view class="head">
        <h1> 可视化大屏展示通用模板</h1>
        <view class="time_now"><span>{{date_now}}</span></view>
    </view>

</template>

<script setup>
import { ref, onMounted } from 'vue';
const dt = ref(new Date());
const date_now = ref('');

onMounted(() => {
    var t = null;
    t = setTimeout(time, 1000); // 开始运行

    function time() {
        clearTimeout(t); // 清除定时器
        dt.value = new Date();
        var y = dt.value.getFullYear();
        var mt = dt.value.getMonth() + 1;
        var day = dt.value.getDate();
        var h = dt.value.getHours(); // 获取时
        var m = dt.value.getMinutes(); // 获取分
        var s = dt.value.getSeconds(); // 获取秒
        date_now.value = y + '年' + mt + '月' + day + '-' + h + '时' + m + '分' + s + '秒';
        t = setTimeout(time, 1000); // 设定定时器，循环运行
    }
})

</script>

<style>
.head{
    background-size: 100%,100%;
    position:flex;
    z-index: 100;
    width: 100vw;
    background-image: url("../assets/head_bg.png");


}
.head h1{
    color: aliceblue;
    text-align: center;
    font-size: 2rem;
    line-height: .8rem;
}


.time_now {
    position: absolute;
    right: 2rem;
    top: 1rem;
    line-height: .75rem;

}

.time_now span {
    color: rgba(255, 255, 255, .7);
    font-size: 1rem;
    padding-right: .1rem;
    justify-content: center;
}
</style>