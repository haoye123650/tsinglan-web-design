# 无限可能的学校 · Try and All is Possible

第十三届中国教育创新年会（场外深研）暨清澜山学校第四届创新教育论坛
2026 年 12 月 5 日 · 东莞 · 清澜山学校

手机优先的像素风活动站。用户打开首页 → 玩一关小游戏 → 创建玩家身份（报名）。

## 页面

| 文件 | 作用 |
|---|---|
| `index.html` | 首页：取景框 + 瞄准环 + 「无限可能的学校」字标，PLAY 入口 |
| `game.html` | LEVEL 1 顶砖合体小游戏（键盘 + 触屏，自包含，无外部依赖） |
| `form.html` | 创建玩家身份 / 报名（数据仅存本机 localStorage，不联网） |
| `story.html` | 新手村介绍：领证后从「进入会场」抵达，介绍学校、论坛缘起与当日流程 |

## assets

- `bg-1205.jpg` — 首页背景
- `logo-infinite-school.png / .webp` — 七字立体字标
- `logo-forum-white.png` — 清澜山第四届创新教育论坛（首页左上）
- `logo-pgy-future.png` — 第十三届中国教育创新年会（首页左下）
- `logo-tsinglan-full.png` — 清澜山学校完整 lockup（首页右上）
- `orbitron-*.woff2` — Orbitron 字重 500/700/900，本地加载不联网
- `favicon.png` — 由校徽裁出

## 说明

- 全站无外部 CDN 依赖，断网也能完整运行。
- 动效全部尊重 `prefers-reduced-motion`。
- 部署：纯静态，任意静态托管直接指向 `index.html` 即可。
