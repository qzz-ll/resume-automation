# 简历在线转化为pdf

## 核心
页面html生成pdf

转化为pdf有两种方法：
1.基于canvas的客户端生成方案
2.基于node.js+puppeteer的服务端生成方案

## 基础使用的模板
使用yarn加载 启动。
    yarn 版本v1.22.10
采用stylus预处理器编写样式
    yarn add stylus
    yarn add stylus-loader

转化为pdf：需要两个模板
1.html2canvas html转化为canvas图片
    yarn add html2canvas 生成版本v@1.0.0-rc.7
2.jspdf 图片转化为pdf
    yarn add jspdf 生成版本v2.1.1


