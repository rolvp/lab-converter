# Lab Converter

一个基于 Flask 的色彩空间转换工具，用于在 CIE Lab 与 RGB 之间进行双向换算，并提供可视化预览与色彩空间科普页面。

## 项目简介

本项目主要面向颜色管理、涂料配色、设计沟通等场景，提供以下能力：

1. Lab 转 sRGB 与 Adobe RGB
2. RGB（sRGB 或 Adobe RGB）转 Lab
3. RGB 同步显示 Hex
4. 色彩空间基础知识与对比说明页面

项目采用 D65 光源与 10° 标准观察者参数进行计算。

## 功能清单

1. Lab -> RGB / Hex
2. RGB -> Lab（可选择参考空间：sRGB 或 Adobe RGB）
3. 双颜色预览框（sRGB、Adobe RGB）
4. 互动式色彩指南页面
5. 前后端分离式接口调用（前端 fetch，后端 JSON 返回）

## 技术栈

1. Python 3
2. Flask
3. NumPy
4. Jinja2 模板
5. 原生 HTML/CSS/JavaScript

## 目录结构

1. main.py：后端入口、路由、色彩转换算法
2. templates/index.html：转换器主页面
3. templates/guide.html：色彩空间科普页面
4. static/css/style.css：页面样式
5. requirements.txt：依赖列表

## 本地运行

1. 安装依赖

```bash
pip install -r requirements.txt
```

2. 启动服务

```bash
python main.py
```

3. 打开浏览器访问

- http://127.0.0.1:5000/

## 页面与路由说明

1. GET /
- 转换器主界面
- 页面文件：index.html

2. GET /color_space_guide
- 色彩空间指南界面
- 页面文件：guide.html

3. POST /convert
- 执行转换逻辑
- 请求体按 mode 区分为两类

### mode = lab2rgb

请求示例：

```json
{
	"mode": "lab2rgb",
	"l": 50,
	"a": 20,
	"b": 10
}
```

返回示例：

```json
{
	"success": true,
	"adobe_rgb": [154, 110, 103],
	"s_rgb": [152, 112, 104]
}
```

### mode = rgb2lab

请求示例：

```json
{
	"mode": "rgb2lab",
	"r": 255,
	"g": 0,
	"b": 0,
	"space": "srgb"
}
```

返回示例：

```json
{
	"success": true,
	"lab": [53.24, 80.09, 67.2]
}
```

## 计算说明

1. 白点参数
- 使用 D65/10° 白点常量：
- Xn = 94.811
- Yn = 100.000
- Zn = 107.304

2. 转换路径
- Lab -> XYZ -> RGB
- RGB -> XYZ -> Lab

3. Gamma 处理
- sRGB 使用标准分段 gamma 公式
- Adobe RGB 使用 gamma 2.2 近似处理

4. 色域处理
- RGB 输出阶段对线性值进行 0 到 1 裁剪后再映射到 0 到 255

## 适用场景

1. 色彩标样沟通（Lab 与屏幕颜色互转）
2. 设计与印刷前的色值换算参考
3. 颜色学习与培训展示
4. Web 前端颜色值快速换算

## 注意事项

1. 不同显示器、系统色彩管理设置会影响实际观感。
2. Adobe RGB 数值在非广色域设备上可能与预期有差异。
3. 当前结果适合工程参考，不替代专业分光仪与完整 CMS 流程。
4. 开发模式启动时使用 Flask debug，生产环境请使用 WSGI 服务器（例如 gunicorn）并关闭 debug。

## 依赖

见 requirements.txt：

1. flask
2. numpy
3. gunicorn

## 版权

页面底部版权信息显示为：

- Copyright 3M All Rights Reserved.
