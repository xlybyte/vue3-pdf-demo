# pdfjs渲染使用

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
