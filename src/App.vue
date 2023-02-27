<template>
  <div>
    <iframe class="iframe" v-if="isMobile" src="https://video.c-smart.cn/monitorh5/" frameborder="0"></iframe>
    <Home v-else />
  </div>
  
</template>

<script setup>
import { ref } from 'vue'
import Home from './views/index.vue'
const isMobileHandle = () => {
    let flag = navigator.userAgent.match(
            /(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i
    );
    return flag;
}
const isMobile = ref(isMobileHandle())
</script>

<style>
html {
  font-size: 100px !important;
}
body {
  margin:0;
  padding:0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  font-size: 30px;
}
.iframe {
  width: 100vw;
  height: 100vh;
}
</style>
