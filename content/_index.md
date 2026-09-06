---
# 头部元信息
# 将主页标题留空，即可使用网站标题
title: ''
# 页面描述
summary: ''
# 页面构建日期
date: 2026-09-06
# 页面类型  关键声明，启用 HugoBlox 落地页布局
# HugoBlox 据此套用「落地页」布局模板，按 sections 顺序渲染区块（portfolio/contact 是其他可选类型）
# landing（落地页）
# portfolio（项目展示页）
# contact（联系页）
type: landing

# 页面由 sections 列表中的区块（block）按顺序拼接渲染。导航菜单里的「项目 / 技能 / 经历 / 博客 / 联系」锚点就对应各区块的 id。
# 区块列表，数组顺序 = 页面从上到下的显示顺序
sections:
  # Developer Hero - Gradient background with name, role, social, and CTAs
  # 开发者英雄区块，展示姓名、角色、社交链接、状态图标、滚动字幕、CTA按钮等
  # 使用「开发者首屏」区块模板 
  # 区块 id 为 hero，对应导航菜单里的「首页」锚点
  - block: dev-hero
    id: hero
    content:
      # 读取 data/authors/me.yaml（头像、姓名、社交链接、状态徽章）
      username: me
      # 名字上方的问候语
      greeting: "你好，我是"
      # 显示头像右下角的 💻 徽章
      show_status: true
      # 底部向下滚动的鼠标滚轮指示动画
      show_scroll_indicator: true
      # 打字机动画
      typewriter:
        # 是否启用打字机动画
        # 注意：开启时会顶掉 me.yaml 的 role 显示。
        enable: true
        # 打字机动画的固定前缀
        prefix: "我构建"
        # 打字机动画的字符串列表
        strings:
          - "全栈网络应用"
          - "可扩展的API"
          - "漂亮的用户界面"
          - "开源工具"
          - "博客"
        # 打字机动画的打字速度（毫秒）
        type_speed: 70
        # 打字机动画的删除速度（毫秒）
        delete_speed: 40
        # 打字机动画的暂停时间（毫秒）
        pause_time: 2500
      # 行动按钮   
      cta_buttons:
        - text: View My Work
          # 页内锚点跳转
          url: "#projects"
          # 按钮图标
          icon: arrow-down
        - text: Get In Touch
          url: "#contact"
          icon: envelope
    # 设计选项
    design:
      # 居中对齐
      # 可选值：centered, left, right
      style: centered
      # 头像形状
      # 可选值：circle, square
      avatar_shape: circle
      # 是否启用入场动画
      animations: true
      # 背景设置
      background:
        # 背景颜色
        # 可选值：light, dark
        color:
          light: "#fafafa"
          dark: "#0a0a0f"
      # 内边距
      # 可选值：6rem, 4rem, 2rem, 1rem
      spacing:
        padding: ["6rem", "0", "4rem", "0"]
  
  # Filterable Portfolio - Alpine.js powered project filtering
  # 可过滤的项目组合 - 由Alpine.js驱动的项目过滤功能
  # 项目展示区块，展示最近的项目
  - block: portfolio
    # 区块 id 为 projects，对应导航菜单里的「项目」锚点
    id: projects
    content:
      # 项目展示区块的标题
      title: "精选项目"
      # 项目展示区块的副标题
      subtitle: "最近构建的项目，展示我的技能和经验"
      # 项目展示区块的项目数量
      # 可选值：0-100 (0 表示展示所有项目)
      count: 0
      # 从 content/projects/ 目录拉取项目文章。
      filters:
        folders:
          - projects
      # 项目展示区块的筛选按钮
      # 可选值：All, Full-Stack, Frontend, Backend
      buttons:
        # 按钮文字
        - name: All
          # 匹配的分类标签（'*' = 全部） 点击按项目的 category/tag 过滤卡片
          tag: '*'
        - name: 全栈
          tag: 全栈
        - name: 前端
          tag: 前端
        - name: 后端
          tag: 后端
        - name: 大数据
          tag: 大数据
      # 默认选中的筛选按钮索引（默认选中第 0 个按钮（All））
      default_button_index: 0
      # 归档链接配置——项目数超过 count 时自动显示「Browse All」链接。
      # Archive link auto-shown if more projects exist than 'count' above
      # archive:
      #   enable: false  # Set to false to explicitly hide
      #   text: "Browse All"  # Customize text
      #   link: "/work/"  # Custom URL
    # 设计选项
    design:
      # 项目展示区块的项目卡片列数
      # 可选值：1-4
      columns: 3
      # 背景设置
      background:
        # 背景颜色
        # 可选值：light, dark
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      # 内边距
      # 可选值：4rem, 2rem, 1rem
      # 项目展示区块的项目卡片间距
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # Visual Tech Stack - Icons organized by category
  # 技术栈可视化 - 按分类组织图标
  - block: tech-stack
    # 区块 id 为 skills，对应导航菜单里的「技能」锚点
    id: skills
    content:
      username: me
      title: "技术栈"
      subtitle: "构建的技术与常用技能"
    design:
      style: grid
      show_levels: false
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # resume-experience 经历时间线
  # 数据来源：data/authors/me.yaml 的 experience + education 字段（模板原生支持，无需硬编码 items）
  - block: resume-experience
    # 区块 id 为 experience，对应导航菜单里的「经历」锚点
    id: experience
    content:
      username: me
      title: "经历"
      # 日期格式
      # 可选值：Jan 2006, Jan 2006, 2006-01-01
      date_format: Jan 2006
    design:
      columns: '1'
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # 最新博客文章
  # collection 博客列表
  - block: collection
    # 区块 id 为 blog，对应导航菜单里的「博客」锚点
    id: blog
    content:
      title: 博客文章
      subtitle: '关于网页开发、技术等方面的思考'
      text: ''
      # 从 content/blog/ 拉文章；默认排除置顶文章。
      # 可选值：true, false
      filters:
        folders:
          - blog
        # 不排除置顶文章。
        exclude_featured: false
      # 只显示最新 3 篇。   
      count: 3
      # 按日期降序排序。
      order: desc
    design:
      # 博客文章卡片视图，默认卡片视图。
      # 可选值：grid, card
      # 可选值：1-4
      view: card
      columns: 3
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # 联系部分
  - block: contact-info
    # 区块 id 为 contact，对应导航菜单里的「联系」锚点
    id: contact
    content:
      title: 联系我们
      subtitle: "联系我们，一起构建神奇的项目"
      text: |-
        我总是对了解新项目和机会很感兴趣。无论您是想寻求合作，还是只是想打个招呼，都欢迎随时联系我们！
      email: service@011231.xyz
      autolink: true
      # prospective:
      #   title: "下载简历"
      #   text: "查看我的详细工作经历和技能列表。"
      #   button:
      #     text: "下载 Resume"
      #     url: uploads/resume.pdf
    design:
      columns: '1'
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "1rem", "0"]
---
