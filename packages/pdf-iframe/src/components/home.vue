<template>
  <div class="iframebox">
    <PdfIframe :url="url"/>
  </div>
</template>
<script setup>
import { ref } from 'vue';
import PdfIframe from './pdf-iframe/index.vue'

const url = ref('/test2.pdf')

</script>
<style scoped>
.iframebox {
  height: 100vh;
}
</style>