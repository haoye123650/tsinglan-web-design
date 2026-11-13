# HANDOFF｜无限可能的学校 · 活动站交接说明

> 给下一位开发者（Cursor）的完整上下文。看完这份可以直接开工，不需要再问需求。
> 最后更新：2026-11-13

---

## 1. 项目是什么

清澜山学校 2026 年 12 月 5 日活动的**手机优先**单页活动站：

> **第十三届中国教育创新年会（场外深研）暨清澜山学校第四届创新教育论坛**
> 主题：无限可能的学校 / 英文标语（slogan）：**Try and All is Possible**
> 时间：2026.12.05 · 08:30 签到，09:00 开场 · 主会场：水木剧场

**受众与场景（决定了所有设计取舍）**：现场约 500 位来宾，**设备尺寸高度分散**（iPhone SE 到 iPad 到桌面投屏），会在一两分钟内打开、玩一关、填个名字。所以：

- 手机优先，**任何宽度/高度下内容都不能被裁、不能折行错位**
- 无外部 CDN、无网络依赖，断网也能跑（会场网络不可靠）
- 全站单文件自包含（HTML + 内联 CSS/JS + 本地 assets）

---

## 2. 两个目录的分工（**最容易搞混，先看这条**）

| 位置 | 作用 |
|---|---|
| `/Users/samz/WorkBuddy/Tsinglan web page/` | **工作台**。多出的草稿：`index-p.html`（P 版，已弃用）、`.workbuddy/backup/`（历史版本与生成脚本） |
| `/Users/samz/tsinglan-web-design/` | **部署仓库**（git）。只含线上真实使用的文件 |

- 线上仓库 = `origin.cursor.com/sam934/tsinglan-web-design.git`，分支 `main`
- **改动流程**：先在仓库里改 → 提交 → 推送；工作台那份不是必需的，若要同步再拷回去
- 推送注意：这台机器上有代理，直接 `git push` 会卡住，用
  ```bash
  git -c http.proxy= -c https.proxy= push origin main
  ```
  凭据走 macOS 钥匙串（`osxkeychain`），无需手动登录

---

## 3. 页面流程与文件职责

```
index.html ──PLAY──▶ game.html ──通关 / SKIP──▶ story.html ──查看今日任务──▶ schedule.html ──查看十二场工作坊──▶ workshops.html
```

| 文件 | 作用 | 关键结构 |
|---|---|---|
| `index.html` | 首页：取景框 + 全息瞄准环 + 七字立体字标 + PLAY | `.hud--top` `.core` `.brands` `.title__img` `.hud--bottom` |
| `game.html` | LEVEL 1「顶砖合体」小游戏，键盘/触屏都能玩 | `.pad` `.dpad` `.jump` `.skip` |
| `story.html` | 新手村介绍：学校、论坛缘起、研讨命题。底部按钮去日程 | `.brand` `.head` `.cta` |
| `schedule.html` | 今日任务：当天时间表。底部按钮去工作坊 | `.brand` `.head` `.cta` |
| `workshops.html` | 十二场工作坊。提前一两天报名、报满即止。地点名额和报名接口还没接 | `.brand` `.head` `.step` |
| `index-p.html` | **P 版旧方案，已弃用**，不在仓库里，不要改也不要引用 | — |

**game.html 的 SKIP 是刻意留的**：现场有人不想玩，必须能一步跳过，不能卡在游戏页。

---

## 4. 资产清单（`assets/`，全部仓库内已有，直接引用即可）

| 文件 | 用途 | 尺寸 |
|---|---|---|
| `bg-1205.jpg` | 首页背景（会场/校园氛围图），1600×900 | 163 KB |
| `logo-infinite-school.png` / `.webp` | **「无限可能的学校」七字立体字标**，首页主视觉 | 1760×266 |
| `logo-forum-white.png` / `.webp` | 论坛 white 版 logo（首页左上第一枚） | 1600×459 |
| `logo-pgy-future.png` / `.webp` | 「未来 / 第十三届中国教育创新年会」white 版（首页左上第二枚） | 1200×286 |
| `logo-tsinglan-full.png` / `.webp` | 清澜山学校完整 lockup（首页右上） | 1200×619 |
| `orbitron-500/700/900.woff2` | 英文与数字的字体，本地加载 | 各约 6 KB |
| `favicon.png` | 由校徽裁出的 64px 图标，各页都挂了 | 64×64 |

**原始素材位置（本机，仅作替换时参考，不在仓库里）**

- `/Users/samz/Desktop/第四届创新论坛logo-white/高质量可放大logo - white 1000ppi.png`
- `/Users/samz/Desktop/第四届创新论坛logo-white/蒲公英-未来logo.png`
- `/Users/samz/Desktop/清澜山学校logo最新-02.png`
- `/Users/samz/WorkBuddy/2026-11-12-10-02-06/` — 官方 logo 资产包（含透明底/白底/方形版 + 品牌色说明 md）

> 注意：这些原图是**四字版「无限游戏」**。首页用的七字「无限可能的学校」是**按同风格程序化复刻**的（深紫挤出 + 青色轮廓线 + 字面渐变），生成脚本已备份在 `.workbuddy/backup/mklogo-infinite-school.py`。
> **如果拿到官方七字版，直接替换 `logo-infinite-school.png/.webp` 这两个文件即可，版式不用动**（宽高比按 1760×266 保持一致）。

---

## 5. 设计 token

```css
/* 颜色 */
--ink:      #04030A;  /* 底色，近黑偏紫 */
--violet:   #3B227B;  /* 品牌紫 */
--violet-l: #6B4EC4;
--gold:     #C79B6E;  /* 金，取景框与主按钮 */
--gold-l:   #F0D6B6;  /* 亮金，标题与高光文字 */
--ice:      #9FD3EC;  /* 冰蓝，数据读数与细线 */
--paper:    #F2ECE6;  /* 正文暖白 */
--neon:     #FF6FB5;  /* 霓虹粉，仅字标与少量信号点 */
--neon-hot: #FF2E9A;

/* 字体 */
--font-display / --font-ui : PingFang SC / Hiragino Sans GB / Microsoft YaHei   /* 中文 */
--font-led / --font-mono   : Orbitron（本地 woff2）                              /* 全部英文与数字 */
```

**工艺语言（四个页面共用，改版时必须保持）**

- **切角**：主按钮用 `clip-path:polygon()` 切八角（`8px`），不是 `border-radius`
- **金砖质感**：`linear-gradient(180deg,#E8BE4C,#C98F2B 62%,#A96F16)` + `box-shadow:0 4px 0` 做厚度，按下时 `translateY(4px)` 且去掉阴影
- **取景框**：四角 `L` 形角标 + 细线，模拟 HUD 观感
- **微排版**：大写 + 宽字距的英文小标（8–9px），永远配一个右侧后缀标签
- **动效**：入口一次编排（`.in` `.in1..in6`），其余只在交互时响应；全部包在 `@media (prefers-reduced-motion:reduce)` 里关掉

---

## 6. 踩过的坑（**改代码前请先读，否则会改坏**）

### 6.1 首页主标题是**一张图**，不是文字

`.title__img` 引用 `logo-infinite-school.png`，宽度 `var(--lw)`。

**原因**：早期用 CSS 文字标题（`19vw` 字号 + 发光描边），在窄屏七个字会折成两行 —— 现场出现过。改成图片后物理上不可能折行。
**所以**：不要把它改回文字，也不要用 `vw` 直接排长中文标题。

### 6.2 单屏页 (`body{overflow:hidden}`) 必须按**视口高度**做断点

首页内容一超高就被直接裁掉、连滚动条都没有。断点顺序（改任何间距都要回归这几档）：

```css
@media (max-height:760px){ /* 短屏再收按钮和标题间距 */ }
@media (max-height:680px){ /* 收紧标题/按钮/间距，顶栏 logo 降一档 */ }
@media (max-height:640px){ /* 再收一档 --lw 与间距 */ }
```

首页跑马灯已按要求删除。日期改放在底栏读数里，不再靠滚动字幕。

### 6.4 游戏页底部三控件用**三栏网格**，不能用 flex + auto margin

`.pad { display:grid; grid-template-columns:1fr auto 1fr }`，`.dpad` 靠左、`.skip` 居中、`.jump` 靠右。
**原因**：方向键（两个键）比 JUMP 宽，flex 下 `margin:auto` 算出的中点比页面中线偏右约 20px。

### 6.5 顶栏微排版的字距是从**字宽反算**的，改文案必须重算

`slogan` 从 `INFINITE GAME`（13 字符）换成 `TRY AND ALL IS POSSIBLE`（23 字符）后，直接替换会撑破容器。现在的值：

| 位置 | 字号 / 字距 | 后缀 |
|---|---|---|
| story / form `.brand` | `9px` / `.12em` + `white-space:nowrap` | `7px` / `.12em` |
| form `.card__top`（通行证） | `7.5px` / `.12em` + `nowrap` | `6px` / `.1em` |

配 `@media (max-width:300px){ .brand em, .card__top em { display:none } }` 兜极窄屏（折叠屏外屏）。320px 下实测文字与后缀间隙 20–23px。

**再改文案时的正确做法**：用 fontTools 读 `assets/orbitron-700.woff2` 的 `hmtx` 算 advance（`TRY AND ALL IS POSSIBLE` = 15.412em），先算宽度再定字号，别试错。

### 6.6 顶栏 logo 宽度下限不能删

```css
.brands__logo--forum{width:clamp(108px,42vw,320px)}
.brands__logo--pgy{width:clamp(88px,34vw,262px)}   /* 刻意比上一枚窄，形成台阶 */
.brands__logo--ts{width:clamp(78px,30vw,252px)}
```

下限是给 <290px 的超窄屏留的，防止左右两侧互相顶到。「未来」那枚笔画重，所以刻意窄一档，视觉重量才平衡。

### 6.7 文案必须和实际流程一致

首页 PLAY 下的提示是「通关后进入会场」。代号报名页已删，通关和 SKIP 都直接进入 story。**按钮说什么，用户就经历什么。**

---

## 7. 怎么自测（这台机器上验证页面的办法）

无头 Chrome 的 `--window-size` **宽度最小 500**，直接截 320/390 会失真。用 iframe 拼多个宽度再截：

```bash
# 1. 建一个承载页，把要测的尺寸并排放
cat > /tmp/_chk.html <<'EOF'
<!DOCTYPE html><html><head><meta charset="utf-8"><style>html,body{margin:0}iframe{display:block;border:0}</style></head><body>
<iframe src="index.html" width="320" height="568"></iframe>
<iframe src="index.html" width="375" height="667"></iframe>
<iframe src="index.html" width="390" height="844"></iframe>
<iframe src="index.html" width="430" height="932"></iframe>
</body></html>
EOF

# 2. 截图（高度按 iframe 总高给）
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu \
  --user-data-dir=/tmp/wbc --virtual-time-budget=5000 --window-size=460,3100 \
  --screenshot=/tmp/shots.png file:///tmp/_chk.html
```

- 首页有入场动画，截图前给页面 `</body>` 前注入 `<style>.in{opacity:1!important;transform:none!important;animation:none!important}</style>`，否则截到空白
- **必测矩阵**：`320×568`、`375×667`、`360×640`、`390×844`、`414×736`、`430×932`、`1440×900`
- **判定标准**：`document.documentElement.scrollWidth == innerWidth`（无横向溢出）+ 底部读数/跑马灯完整可见（无竖向裁切）
- 测文字是否被挤：别用 `scrollWidth`（会被夹到容器宽度而看不出溢出），用 `Range.getBoundingClientRect()` 取文字节点右边界，再和相邻元素左边界比

---

## 8. 当前状态 / 待办

**已完成并上线**：首页到工作坊的流程、官方 logo 顶栏、slogan 全站统一、游戏页金砖 SKIP、全设备自适应收紧、favicon、README。代号报名页已删除。

**待办（按优先级）**

1. **`workshops.html` 已从日程拆出**。日程底部按钮是「查看十二场工作坊」。报名是提前一两天、报满即止，不是当天门口计数。地点和名额仍待补。席位必须记在所有人共用的服务端，一个人只能报一场。
2. **story 之后还要加页**（用户明确说后面会加），所以 story 底部**不要**再做「回到起点」这类闭环按钮，留成可续接的出口。
3. 字标工艺还有提升空间：现在的七字版渐变少了定稿那种金属反射感，若拿到官方图直接换文件（见 §4）。

---

## 9. 硬性约束（不要做）

- ❌ 不要引入 CDN / 外部字体 / 网络请求 —— 会场可能断网
- ❌ 不要用 `vw` 排长中文标题（折行根源，见 §6.1）
- ❌ 不要删 `@media (max-height:*)` 那几个断点
- ❌ 不要给 story 加「回到起点」闭环
- ❌ 不要"顺手修正"文件名里的中英混用（`un处理able` 这类是用户习惯）
- ❌ 不要动 `index-p.html`（弃用件）
- ✅ 中文注释、中文文案，代码风格跟现有文件保持一致（紧凑、单行声明、区块用 `/* ---------- xxx ---------- */` 分隔）
