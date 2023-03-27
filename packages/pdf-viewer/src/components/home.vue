<template>
  <div>
    <div class="pan">
      <button @click="printFn">打印pdf</button>
    </div>
    <PdfViewer :url="url"/>
  </div>
</template>
<script setup>
import { ref } from 'vue';
import PdfViewer from './pdf-viewer/index.vue'

const url = ref('/test2.pdf')

function printFn(){
  window.print()
}
</script>
<style scoped>
@media print {
  .pan {
    display: none;
  }
}
</style>