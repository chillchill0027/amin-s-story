# 阿明的故事 — 工作流文档

## 项目概述

**项目名称**：裂缝中的光 — 阿明的故事  
**项目类型**：交互式网页故事  
**技术栈**：纯 HTML/CSS/JavaScript（无框架依赖）  
**设计风格**：Apple 风格暗色主题  
**部署方式**：GitHub Pages  

---

## 工作流程

### 阶段一：故事创作与完善

#### 1.1 初始故事结构
- **Act 1**：童年记忆（大马士革）
- **Act 2**：逃亡之路（地中海）
- **Act 3**：新生活（德国）
- **Act 4**：成长与反思
- **Ending**：26岁的阿明

#### 1.2 场景补充任务
根据用户要求，补充了五个关键场景：

| 场景 | 状态 | 关键内容 |
|------|------|----------|
| Act1 生日面包插蜡烛 | ✅ 已完成 | 蜡烛是红色的，面包是白色的，蜡油滴下来，浸在面包皮上，像一滴眼泪 |
| Act2 萨拉洗发水味 | ✅ 已完成 | 她的头发有淡淡的洗发水味——妈妈出门前给她洗了最后一次头 |
| Act2 妈妈的沉默 | ✅ 已完成 | 妈妈获救后"一整天一整天不说话" |
| Act3 收银员退后一步 | ✅ 已完成 | 她看了他一眼，往后退了一步 |
| Act3 格尔达从数学开始 | ✅ 已完成 | 那我们从数学开始 |
| Act4 阿明日记全文 | ✅ 已完成 | 完整的日记段落，连接所有事件 |
| Ending 26岁现状 | ✅ 已完成 | 还在学德语，报了一个远程的工程预科课程 |
| Ending 梦见萨拉 | ✅ 已完成 | 每次醒来，手都是空的 |

---

### 阶段二：网页开发

#### 2.1 技术实现
- **单文件架构**：所有代码集中在 `index.html`
- **响应式设计**：适配桌面和移动端
- **滚动动画**：使用 Intersection Observer API
- **导航系统**：侧边导航点 + 平滑滚动

#### 2.2 设计系统
```css
/* 色彩系统 */
--bg: #000000;           /* 纯黑背景 */
--bg-elevated: #1c1c1e;  /* 卡片背景 */
--text: #f5f5f7;         /* 主文本 */
--text-secondary: #a1a1a6; /* 次要文本 */
--accent: #c9a96e;       /* 金色强调色 */

/* 字体系统 */
--font-family: "SF Pro SC", "SF Pro Display", ...;
--font-serif: "Noto Serif SC", "Source Han Serif CN", ...;
```

#### 2.3 交互功能
- **视差滚动**：Hero 区域的视差效果
- **渐入动画**：内容区块的滚动渐入
- **导航高亮**：当前章节的导航点高亮
- **平滑滚动**：点击导航点的平滑跳转

---

### 阶段三：版本控制与部署

#### 3.1 Git 工作流
```bash
# 初始化仓库
cd /Users/chill/Downloads/amins-story
git init
git add .
git commit -m "阿明的故事：一个叙利亚少年的人生轨迹"

# 连接远程仓库
git remote add origin https://github.com/chillchill0027/amin-s-story.git
git branch -M main
git push -u origin main
```

#### 3.2 遇到的问题与解决方案

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| `fatal: not a git repository` | 当前目录不是 Git 仓库 | 切换到正确的项目目录 |
| `Host key verification failed` | SSH 主机密钥未添加 | `ssh-keyscan github.com >> ~/.ssh/known_hosts` |
| `Permission denied (publickey)` | SSH 密钥未配置 | 改用 HTTPS 方式推送 |
| `rejected: main -> main (fetch first)` | 远程仓库已有内容 | 使用 `--force` 强制推送 |

#### 3.3 最终部署命令
```bash
# 切换到项目目录
cd /Users/chill/Downloads/amins-story

# 设置远程仓库（HTTPS）
git remote set-url origin https://github.com/chillchill0027/amin-s-story.git

# 强制推送
git push -u origin main --force
```

---

## 项目文件结构

```
amins-story/
├── index.html          # 主页面（85KB）
├── images/             # 图片资源目录
│   ├── image1.jpg
│   ├── image2.jpg
│   └── ...
├── .git/               # Git 仓库
├── .DS_Store           # macOS 系统文件
└── workflow.md         # 本文档
```

---

## 关键代码片段

### 4.1 滚动渐入动画
```javascript
// Intersection Observer 配置
const revealObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
    }
  });
}, { threshold: 0.15, rootMargin: '0px 0px -40px 0px' });
```

### 4.2 视差滚动效果
```javascript
window.addEventListener('scroll', () => {
  requestAnimationFrame(() => {
    const hero = document.getElementById('hero');
    if (hero) {
      const scrollY = window.scrollY;
      const opacity = 1 - (scrollY / window.innerHeight) * 1.5;
      const translateY = scrollY * 0.3;
      hero.style.opacity = Math.max(0, opacity);
      hero.style.transform = `translateY(${translateY}px)`;
    }
  });
}, { passive: true });
```

### 4.3 导航点高亮
```javascript
const sectionObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const id = entry.target.id;
      navDots.forEach(dot => {
        dot.classList.toggle('active', dot.dataset.section === id);
      });
    }
  });
}, { threshold: 0.3 });
```

---

## 后续优化建议

### 5.1 性能优化
- [ ] 图片懒加载（`loading="lazy"`）
- [ ] CSS 动画优化（使用 `transform` 和 `opacity`）
- [ ] 减少重绘重排

### 5.2 功能增强
- [ ] 添加音频背景音乐
- [ ] 实现章节进度保存
- [ ] 添加分享功能
- [ ] 支持深色/浅色主题切换

### 5.3 内容扩展
- [ ] 添加更多配图
- [ ] 实现多语言支持
- [ ] 添加作者注释

---

## 总结

本项目通过 AI 辅助创作，完成了一个交互式网页故事的开发：

1. **故事创作**：完善了五个关键场景，增强了情感表达
2. **网页开发**：实现了 Apple 风格的暗色主题设计
3. **版本控制**：使用 Git 进行版本管理
4. **部署上线**：成功推送到 GitHub 仓库

**项目地址**：https://github.com/chillchill0027/amin-s-story

---

*文档生成时间：2026-05-20*  
*项目状态：已完成并部署*
