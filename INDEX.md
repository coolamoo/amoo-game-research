# 索引 · 从这个文件进入

这是"赛车游戏原型研究"的记忆仓库。**每次重新打开这个项目，先读这一页**，不用从头翻代码。

- 仓库：`github.com/coolamoo/amoo-game-research`
- 起始：2026-11-19 凌晨 03:27 — 04:41，一个通宵做出三个可玩原型
- 形态：单文件 HTML，浏览器直接打开，零构建
- 最后更新：2026-11-27

## 一、三条阅读路径

| 你有多少时间 | 读什么 |
|---|---|
| 30 秒 | 本页「三、现状快照」 |
| 10 分钟 | `README.md` → `docs/tech-notes.md` |
| 要动手 | `docs/next-steps.md` 挑一条 → `docs/params.md` 查参数 → 改对应 `games/*.html` |

## 二、想查什么，去哪

| 我想知道 | 去这个文件 | 关键词 |
|---|---|---|
| 当时到底做了几个东西、各自在试什么 | `README.md` | 三个原型、技术栈 |
| 什么时候做的、按什么顺序 | `docs/timeline.md` | 03:27 / 03:44 / 04:04 |
| 漂移手感怎么调的（数值） | `docs/tech-notes.md`、`docs/params.md` | gripNormal / gripDrift / nitro |
| 3D 画面为什么发灰 / 怎么调色 | `docs/tech-notes.md` | ACESFilmicToneMapping / Fog |
| 霓虹发光怎么做的 | `docs/tech-notes.md` | CanvasTexture / Bloom |
| 踩过哪些坑 | `docs/tech-notes.md` 第四节 | 坑、结论 |
| 接下去做什么 | `docs/next-steps.md` | 勾选框 |
| 还没想清楚的问题 | `research/questions.md` | 问号条目 |
| 某个术语什么意思 | `docs/glossary.md` | 抓地力 / LOD / GLB |
| 车模有哪些、怎么加载 | `assets/README.md` | glb、静态服务器 |
| 三个原型怎么打开 | `games/README.md` | file:// 、http.server |

## 三、现状快照

| 原型 | 技术 | 状态 |
|---|---|---|
| `games/pixel-racer.html` | Canvas 2D，320×560 竖屏，零依赖 | 可玩，手感基准版 |
| `games/neon-racer-3d.html` | Three.js 0.149 全局构建 | 可玩，霓虹观感 |
| `games/road-racer-3d.html` | Three.js 0.149 ES Module + ACES | 可玩，最接近成品观感 |

一句话结论：**手感在 2D 版定，观感在 3D 版定；调手感改 2D 参数，不要在 3D 里盲调。**

## 四、标签索引

用标签快速定位内容（每个标签后面是文件）：

- `#物理` — `docs/params.md`、`docs/tech-notes.md`
- `#渲染` — `docs/tech-notes.md`（色调映射、雾、CanvasTexture）
- `#素材` — `assets/README.md`
- `#移动端` — `docs/next-steps.md` 二、`research/questions.md` Q2
- `#实验结论` — `README.md` 第四节、`docs/tech-notes.md` 第四节
- `#待办` — `docs/next-steps.md`

## 五、维护规则（重要）

这个仓库能当记忆用，靠的是三条纪律：

1. **做完一件事** → 在 `docs/next-steps.md` 划掉，结论写进 `docs/tech-notes.md`，然后回本页更新「最后更新」日期。
2. **改动了参数** → 同步更新 `docs/params.md`；不要在代码和文档里留两份不一致的数。
3. **新增文件** → 必须在本页「二、想查什么，去哪」加一行，否则半年后没人找得到。

代码可改，文档别欠账。
