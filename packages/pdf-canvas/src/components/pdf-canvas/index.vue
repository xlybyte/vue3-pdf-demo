<template>
  <div ref="pdfRef" class="pdf-view"></div>
</template>
<script setup>
// canvas方式渲染pdf，版本pdfjs-dist": "^2.16.105
// npm install pdfjs-dist@2.16.105
import { ref, watch } from "vue"
import PdfjsWorker from "pdfjs-dist/build/pdf.worker.js?worker"

const props = defineProps({
  url: {
    type: String,
    required: true
  }
})

const pdfRef = ref(null)

watch(
  () => props.url,
  () => {
    setTimeout(() => {
      init()
    }, 1)
  },
  { immediate: true }
)

// 渲染pdf
async function init() {
  const PDFJS = await import("pdfjs-dist/build/pdf.js")
  if (typeof window !== "undefined" && "Worker" in window) {
    PDFJS.GlobalWorkerOptions.workerPort = new PdfjsWorker()
  }
  // 加载文档
  let loadingTask = PDFJS.getDocument({ url: props.url })
  loadingTask.__PDFDocumentLoadingTask = true
  const pdf = await loadingTask.promise
  // 循环渲染每一页
  for (let i = 1; i <= pdf.numPages; i++) {
    const page = await pdf.getPage(i)
    let pixelRatio = 3
    let viewport = page.getViewport({ scale: 1 })
    let divPage = window.document.createElement("div")
    let loading = window.document.createElement("div")
    loading.className = "loading"
    divPage.appendChild(loading)
    // 使用canvas渲染
    let canvas = divPage.appendChild(window.document.createElement("canvas"))
    divPage.className = "page"
    pdfRef.value.appendChild(divPage)
    canvas.width = viewport.width * pixelRatio
    canvas.height = viewport.height * pixelRatio
    let renderContext = {
      canvasContext: canvas.getContext("2d"),
      viewport: viewport,
      transform: [pixelRatio, 0, 0, pixelRatio, 0, 0]
    }
    // page.render(renderContext).promise.then(() => {
    //   divPage.className = "page complete"
    // }).catch(() => {})
    await page.render(renderContext).promise // 一页一页的渲染
    divPage.className = "page complete"
  }
  console.log('pdf页面全部渲染完毕', pdf.numPages, pdf)
}
</script>
<style scoped lang="scss">
.pdf-view {
  position: relative;
  display: block;
  width: 100%;
  height: 100%;
}

// 组件样式
:deep() {
  .page {
    position: relative;
    canvas {
      width: 100%;
    }
    .loading {
      display: block;
      background-image: url("./loading.svg");
      width: 30px;
      height: 30px;
      background-repeat: no-repeat;
      background-size: 100%;
      position: absolute;
      top: 50%;
      left: 50%;
      margin: -15px 0 0 -15px;
    }
    &.complete {
      .loading {
        display: none;
      }
    }
  }
}
</style>
