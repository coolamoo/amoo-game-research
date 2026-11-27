# 赛车游戏原型研究（game-lab）

> **先读 [`INDEX.md`](INDEX.md)。** 那是这个仓库的记忆索引：想查什么、去哪看、现状快照、维护规则。
> 本文件是项目说明，INDEX 是导航。

给"以后的我"看的项目主页：这里放过什么、当时怎么想的、下一次从哪继续。

- 起始：2026-11-19 凌晨 03:27 — 04:41，一个通宵做出三个可玩原型
- 形态：全部是单文件 HTML，浏览器直接打开就能玩，零构建、零依赖安装（Three.js 走 CDN）
- 状态：三个原型均可运行；尚未做过移动端打磨，也未做过完整的车辆物理

## 一、三个原型分别在试什么

| 文件 | 时间 | 技术 | 当时想试的点 |
|---|---|---|---|
| `games/pixel-racer.html` | 03:27 | 纯 Canvas 2D，无任何库 | 像素风的漂移手感：用最小实现验证"像素画＋速度感"能不能成立，不借引擎 |
| `games/neon-racer-3d.html` | 03:44 | Three.js 0.149（`three.min.js` 全局构建） | 3D 霓虹赛道的第一版：环路赛道、发光边缘、相机跟随 |
| `games/road-racer-3d.html` | 04:04 | Three.js 0.149（ES Module 版本） | 同一套手感的第二次实现：改用模块化写法、ACES 色调映射、更接近真实公路的观感 |

三条线的顺序说明当时的判断：**先用 2D 把"漂移好不好玩"这件事验证掉，再上 3D 解决观感**。2D 版负责手感，3D 版负责氛围。

## 二、素材

| 文件 | 说明 |
|---|---|
| `assets/car.glb` | 主用车辆模型，325 KB |
| `assets/cars/hypercar.glb` | 超跑 |
| `assets/cars/monster_orange.glb` | 大脚车（橙色） |
| `assets/cars/fire_red.glb` | 红色跑车 |
| `assets/cars/police_classic.glb` | 复古警车 |
| `assets/cars/icecream_pink.glb` | 粉色冰淇淋车 |
| `assets/cars/agata.glb` | 另一款车 |

模型都是 300 KB 上下的 GLB，适合网页直载。**注意**：`file://` 下浏览器会拦 GLB 的跨源读取，本地测试要起个静态服务器：

```bash
cd game-research && python3 -m http.server 8080
# 浏览器打开 http://localhost:8080/games/road-racer-3d.html
```

`pixel-racer.html` 不加载外部模型，双击就能玩。

## 三、操作

三个原型都支持键盘 + 指针（`keydown` / `pointerdown`），方向键或 WASD 控制，具体键位见各文件内注释。移动端触控只是接到同一套输入上，没有做专门的手感调校。

## 四、已知的坑与结论

1. **手感来自 2D 版，不是 3D 版。** 漂移的判定、抓地力衰减、氮气节奏都是在 `pixel-racer` 里调出来的，3D 版是把同一套参数搬过去，所以 3D 版调手感时应该改 2D 的参数再回灌，不要在 3D 里盲调。
2. **Three.js 两种引入方式都试过了。** `neon-racer` 用全局 `three.min.js`（写法短、加载快），`road-racer` 用 ES Module（能引 `examples/jsm`，代价是要跑在 http 下）。
3. **色调映射决定"像不像游戏"。** `road-racer` 加了 `ACESFilmicToneMapping` 才不像玩具，之前直出的颜色发灰。
4. **发光感靠贴图，不靠后处理。** 霓虹赛道用 `CanvasTexture` 画发光带，比上 Bloom 便宜太多，也更容易在低端机跑起来。

## 五、下一次从哪继续

待研究问题（按优先级）：

1. **车辆物理**：现在是简化的街机手感，要不要引入质量/悬挂/轮胎摩擦曲线？先从 2D 版试，跑通了再搬 3D。
2. **移动端**：竖屏、虚拟摇杆、帧率上限。目前触控只是"能按"，不是"好玩"。
3. **GLB 管线**：7 个模型都是现成的，但没做过减面、贴图压缩、LOD。要不要统一压到 150 KB 以内。
4. **赛道生成**：现在赛道是手写坐标，试过程序化生成没有？
5. **音效**：引擎声随转速变调这一条一直没做。

## 六、怎么用这个仓库

- 每次重新捡起这个项目，先读 `INDEX.md`（导航），再按它指的路走：`docs/timeline.md`（时间线）→ `docs/tech-notes.md`（技术细节）→ `docs/next-steps.md`（挑一件事）。
- 做完一件事，往 `docs/next-steps.md` 里划掉并把结论补进 `docs/tech-notes.md`。
- 原型不删，做新版本另存（`pixel-racer-v2.html` 这种），保留可对照的旧版本。
