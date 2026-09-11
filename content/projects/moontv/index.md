---
title: "MoonTV 影视聚合播放器"
date: 2025-07-18
summary: "基于 Next.js 14 的跨平台影视聚合播放器，多源聚合搜索、HLS 在线播放、收藏与播放记录多端同步"
tags:
  - 全栈
  - 后端
  - 自部署
tech_stack:
  - Next.js 14
  - TypeScript
  - Redis
  - Docker
links:
  - type: github
    url: https://github.com/DreamPathway/MoonTV
    label: Code
  # 如有自部署的线上地址，取消注释并填入：
  - type: live
    url: https://tv.011231.xyz/
    label: Demo
featured: true
status: "Live"
role: "独立部署与维护"
duration: "持续迭代"
team_size: 1
highlights:
  - "数十个资源站一次搜索，全源结果即时聚合"
  - "HLS.js + ArtPlayer 流畅播放，实验性自动跳过切片广告"
  - "收藏与播放记录云端同步，支持 Redis / D1 / Upstash 三种存储"
  - "PWA 离线缓存，Docker / Vercel / Cloudflare 三种部署方式"
---

开箱即用的影视聚合播放器：输入片名一次搜索，聚合数十个免费资源站的结果，直接在线播放，收藏和观看进度跨设备同步——自托管，全端适配。

## 概述

看片资源的痛点从来不是"没有源"，而是源太散：不同资源站各自搜索、画质参差、进度互不相通。MoonTV 用一个 Web 应用把这些源聚合起来——前端 Next.js 14 App Router + Tailwind CSS，播放器集成 ArtPlayer 与 HLS.js，存储层抽象出 localStorage / Redis / Cloudflare D1 / Upstash 四种后端，从单机自用到多账户同步都能覆盖。

## 核心功能

### 聚合搜索
- **多源并发**：内置数十个免费资源站，一次输入立刻返回全源结果
- **详情聚合**：剧集列表、演员、年份、简介等完整信息一页展示

### 播放体验
- **HLS 流媒体**：HLS.js 处理 m3u8 切片，ArtPlayer 提供统一的播放控制
- **智能去广告**：自动跳过视频中的切片广告（实验性）
- **播放记忆**：继续观看自动定位到上次进度

### 数据同步
- **多账户隔离**：账号体系下各用户收藏、播放记录互不干扰
- **跨端同步**：Redis / D1 / Upstash 云端存储，手机、平板、电脑进度无缝衔接
- **管理后台**：站长可管理用户与站点配置

### 多端适配
- **响应式布局**：桌面侧边栏 + 移动端底部导航
- **PWA**：离线缓存、安装到桌面/主屏，移动端接近原生体验
- **AndroidTV**：适配电视端使用场景

## 架构

```
┌────────────────────────────────────────┐
│           Next.js 14 (App Router)      │
│  搜索聚合页 │ 详情页 │ 播放页 │ 管理后台  │
└───────────────┬────────────────────────┘
                │ 存储抽象层（统一接口）
   ┌────────────┼────────────┬────────────┐
┌──▼─────┐ ┌────▼───┐ ┌──────▼───┐ ┌──────▼─────┐
│localSt.│ │ Redis  │ │ CF D1    │ │ Upstash    │
│(单机)   │ │(Docker)│ │(CF Pages)│ │(Vercel)    │
└────────┘ └────────┘ └──────────┘ └────────────┘
```

- **存储抽象**：四种存储后端实现同一套数据接口，按部署环境选择，业务代码无感知
- **资源站配置化**：`config.json` 声明采集源，增删源不改代码
- **部署矩阵**：Docker（含 Redis 全家桶 Compose）、Vercel（Upstash）、Cloudflare Pages（D1 + `nodejs_compat`），各自有最佳实践文档

## 挑战与解决

### 多源搜索的延迟与稳定性
**问题**：数十个资源站并发请求，慢源会拖垮整体响应，个别源宕机还会报错。

**解决**：并发聚合 + 单源超时熔断，单个源失败不影响其余结果返回，全局错误指示器提示降级状态。

### 四种存储后端的统一
**问题**：localStorage、Redis、D1、Upstash 的数据模型与能力差异大，逐处 if/else 会迅速腐化。

**解决**：抽象出统一的存储层接口（收藏、播放记录、用户），各后端独立实现，部署时通过 `NEXT_PUBLIC_STORAGE_TYPE` 一个环境变量切换。

### 跨平台播放兼容性
**问题**：浏览器、移动端 WebView、电视大屏对 HLS 与编解码支持参差。

**解决**：HLS.js 统一处理 m3u8 拉流与分片调度，ArtPlayer 抹平各端播放器 UI 差异，PWA 与响应式布局覆盖剩余的端侧差异。

## 技术栈细节

**前端**
- Next.js 14 App Router + TypeScript
- Tailwind CSS 3 响应式主题
- ArtPlayer + HLS.js 播放内核

**存储**
- localStorage：零依赖单机模式
- Redis（Docker 自建）/ Upstash（Serverless）：多账户同步
- Cloudflare D1：Pages 部署的配套数据库

**工程质量**
- ESLint + Prettier + Husky 统一规范
- Jest 单元测试
- GitHub Actions 自动构建，Docker 镜像多架构发布

## 成果

- **聚合**：一次搜索覆盖数十个资源站，找片效率数量级提升
- **同步**：收藏与进度跨设备无缝衔接，换设备不用重找进度
- **成本**：Vercel / Cloudflare 免费额度可跑，Docker 版单容器即可
- **体验**：PWA 安装到主屏，移动端接近原生 App

## 后续计划

- [ ] 弹幕功能
- [ ] 更多存储后端
- [ ] 播放画质偏好记忆
- [ ] 资源站健康度监测面板

---

**项目状态**：✅ 生产运行中  
**GitHub**：[查看源码](https://github.com/DreamPathway/MoonTV)   
**Demo**：[Try it Live](https://tv.011231.xyz/)  
