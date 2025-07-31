<template>
  <div class="title-container">
    <div class="title-text">
      <p>大鹏应急指挥系统</p>
      <span>Dapeng Emergency Command System</span>
    </div>
    <div class="list">
      <ul>
        <li v-for="(item, index) in menu" :key="item" :class="{ active: menuIndex === index }" @click="menuIndex = index; emit('menu', index)">{{ item }}</li>
      </ul>
    </div>
    <div class="time">
      <p>{{ time }}</p>
      <span>{{ date }}</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import dayjs from 'dayjs';
let menu = ['海基监控', '陆基监控', '空基监控', '视频监控', '应急处理'];
let menuIndex = ref(0);
let time = ref('00:00:00');
let date = ref('--');
let timer = null;
let emit = defineEmits(['menu']);
const updateTime = () => {
  const now = new Date();
  time.value = dayjs(now).format('HH:mm:ss');
  date.value = dayjs(now).format('YYYY-MM-DD');
};
onMounted(() => {
  updateTime();
  timer = setInterval(updateTime, 1000);
});
onUnmounted(() => {
  if (timer) clearInterval(timer);
});
</script>

<style lang="scss" scoped>
.title-container {
  display: flex;
  width: 100%;
  height: 100%;
  padding: vw(30) vw(20);
  font-size: vw(24);
  color: #fff;
  position: relative;
  align-items: center;
  justify-content: space-between;
  .title-text{
    p {
      font-size: vw(48);
      font-weight: bold;
      margin-bottom: vh(6);
    }
    span {
      font-size: vw(20);
      color: #dadada;
    }
  }
  .time{
    text-align: right;
    font-size: vw(30);
    span{
      font-size: vw(22);
    }
  }
  .list{
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #000000b2;
    border: vw(1) solid #ffffff70;
    padding: vw(12) vw(24);
    border-radius: vw(20);
    position: absolute;
    @include absolute-center;
    ul {
      display: flex;
      gap: vw(20);
      li {
        padding: vw(14) vw(30);
        background-color: #7a7a7a69;
        backdrop-filter: blur(vw(10));
        color: #fff;
        text-align: center;
        border-radius: vw(14);
        cursor: pointer;
        font-size: vw(16);
        transition: background-color 0.3s;
      }
      .active {
        background-color: #146148;
      }
    }
  }
}
</style>