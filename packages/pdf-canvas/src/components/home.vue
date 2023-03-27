<template>
  <div>
    <div class="pan">
      <button @click="printFn">打印pdf</button>
    </div>
    <PdfCanvas :url="url"/>
  </div>
</template>
<script setup>
import { ref } from 'vue';
import PdfCanvas from './pdf-canvas/index.vue'

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