<template>
  <div id="pageContainer">
    <div id="viewer" class="pdfViewer"></div>
  </div>
</template>

<script setup>
// HTML形式渲染pdf,  "pdfjs-dist": "^2.0.943",
// npm install pdfjs-dist@2.0.943
import { defineProps, defineEmits, onMounted } from 'vue'
import pdfjsLib from 'pdfjs-dist/build/pdf.js'
import PdfjsWorker from "pdfjs-dist/build/pdf.worker.min.js?worker"
import { PDFViewer } from 'pdfjs-dist/web/pdf_viewer'
import 'pdfjs-dist/web/pdf_viewer.css'
// pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdn.jsdelivr.net/npm/pdfjs-dist@2.0.943/build/pdf.worker.min.js'
pdfjsLib.GlobalWorkerOptions.workerPort = new PdfjsWorker()
// 给workerPort 赋值，会报错，不过能渲染出来
// 给workerSrc 赋值cdn，则不报错

const props = defineProps({
  url: {
    type: String,
    default: '',
  },
})
const emit = defineEmits(['pdfload'])
onMounted(() => {
  getPdf()
})
async function getPdf() {
  const container = document.getElementById('pageContainer')
  // 实例化pdf视图
  const pdfViewer = new PDFViewer({
    container: container,
  })
  // 加载pdf文件
  const loadingTask = pdfjsLib.getDocument(props.url)
  // 生成文档
  const pdf = await loadingTask.promise
  // 开始绘制到dom
  pdfViewer.setDocument(pdf)
  // 监听pagerendered来实现 判断 是否渲染完成，如果要打印一定要渲染完成后再打印，不然会有空白
  document.addEventListener('pagerendered', function (e) {
    if (e.detail.pageNumber === pdf.numPages) {
      // 渲染完成
      emit('pdfload', pdf)
      // document.removeEventListener('pagerendered')
    }
  })
}
</script>

<style scoped>
#pageContainer {
  position: absolute;
  margin: auto;
  width: 100%;
}

div.page {
  display: inline-block;
}
:deep(.pdfViewer .page) {
  border: none;
}
</style>
