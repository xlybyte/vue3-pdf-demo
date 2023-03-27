# pdfjs渲染使用

## 介绍

本项目演示了如何使用pdf.js进行渲染pdf,一共演示了三种模式，不同的渲染方式使用的pdfjs版本不同，目前发现不同版本它使用方式有差异，所以这里用了不同的版本来演示
1.使用canvas渲染方式
2.使用(dom文字+canvas图标)渲染方式,文字可以复制
3.完全使用官方示例的-完整功能渲染方式

本项目使用的vue3.2，vite为基础

## 运行

项目采用 monorepo 管理

注意：请使用 yarn 作为包管理工具

安装全部项目依赖
```bash
yarn install
```

启动开发项目
```bash
# canvas渲染方式
yarn workspace pdf-canvas run dev
# dom渲染方式
yarn workspace pdf-viewer run dev
# 完全使用pdfjs组件渲染
yarn workspace pdf-iframe run dev
```
