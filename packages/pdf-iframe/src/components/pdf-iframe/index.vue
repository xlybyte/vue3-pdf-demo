<template>
  <iframe
    :src="src"
    frameborder="0"
    width="100%"
    height="100%"
    class="pdf-iframe"
    @load="onLoad"
  ></iframe>
</template>
<script setup lang="ts">
// 2.6.347 版本 是最后一个支持 78版本Chrome浏览器的版本号
import { computed } from 'vue'
const props = defineProps({
  url: {
    type: String,
    required: true,
  },
})
const emit = defineEmits(['pdfload'])
const src = computed(() => {
  return `/web-pdfjs2.6.347/web/viewer.html?file=${props.url}`
})

// iframe加载完成事件
function onLoad() {
  const ifel = document.querySelector<HTMLIFrameElement>('iframe.pdf-iframe')
  if (!ifel) return
  let count = 0
  const timer = setInterval(() => {
    count++
    const h = ifel.contentWindow?.document.querySelector<HTMLDivElement>('#viewer')?.offsetHeight
    if (h && h > 0) {
      emit('pdfload', h) // 触发dom加载完成事件，并抛出文档高度，此时pdf未渲染完成，不可打印
      clearInterval(timer)
    }
    if (count >= 300) {
      // 如果30秒后还未取到高度则终止
      clearInterval(timer)
    }
  }, 100)
}
</script>
<style scoped></style>
