# 技术笔记

三个原型的关键参数、实现取舍、踩过的坑。参数都是从代码里抄出来的，不是回忆。

## 一、`pixel-racer.html`（Canvas 2D，竖屏）

画布 **320×560**，竖屏手机比例，三车道。整个游戏只有 21.7 KB，零依赖。

```js
speed0: 300, speedMax: 980, speedGain: 10        // 速度与加速度
steerAccel: 1500, latMax: 300                    // 转向加速度、横向速度上限
gripNormal: 9, gripDrift: 2.4                    // 抓地力：正常 vs 漂移
yawMax: 0.42                                     // 最大偏航角（视觉倾斜）
driftMinSpeed: 380, driftRate: 0.55, nitroPerDrift: 0.34   // 漂移计分与氮气转化
nitroTime: 2.0, nitroBoost: 1.55, nitroMin: 0.25           // 氮气时长、倍率、最低触发量
spawnStart: 1.0, spawnMin: 0.42                  // 敌车生成间隔，随时间收紧
skidMax: 320                                     // 刹车痕数量上限（环形复用）
```

**核心设计**：漂移不是"按住手刹"，而是 `grip` 从 9 掉到 2.4 —— 抓地力一变，横向位移自己就滑出去，玩家只需要控方向。**漂移量转成氮气**（`nitroPerDrift: 0.34`），所以奖励的是持续漂移，不是踩一下氮气。这两个数值是整个手感的核心，改之前先想清楚。

配色是写死的调色板（草地 `#1b2b1f`、路面 `#282d34`、玩家车 `#e8b100`、6 种敌车色），像素风不需要动态色，写死反而统一。

## 二、`road-racer-3d.html`（Three.js ES Module，横屏）

单位是真实单位：米、米每秒。**这是三版里唯一有物理量纲的**。

```js
roadW: 14, laneCount: 2
accel: 9.5          // 油门加速度 m/s²
brake: 16           // 刹车减速度
engineBrake: 2.2    // 松油门的自然减速
vMax: 78            // ≈ 280 km/h
steerMax: 0.9 rad   // 最大前轮转角
gripLat: 26         // 横向抓地
carLen: 4.4, carWid: 1.85
camDist: 7.4, camHeight: 2.15
traffic: 6, treeCount: 26
fogNear: 60, fogFar: 260
```

渲染层的两个决定：

1. **`ACESFilmicToneMapping`，exposure 1.05** —— 不加这个，直出的颜色发灰，像玩具；加了之后才有"傍晚公路"的感觉。
2. **雾用暖色** `new THREE.Fog(0xc08457, 60, 260)` —— 远景不是变白，是变暖，这是黄昏场景的关键；山的材质 `roughness: 1` 且开 `fog: true`，让远景自动虚化（近实远虚）。

车模当时是从 CDN 直接引的 CC0 低多边形车（`Vuena/free-low-poly-toy-cars-cc0` 里的 `muscle_blue.glb`），后来才换成本地 `assets/cars/*.glb`。

## 三、`neon-racer-3d.html`（Three.js 全局构建）

用 `three.min.js` 全局版本，写法短、不用起 http 服务。发光感靠 **`CanvasTexture` 现画发光带**，不是 Bloom 后处理——便宜、低端机也能跑。这条经验在 `road-racer` 里也沿用了（尾灯、路面反光都是 CanvasTexture）。

## 四、踩过的坑

| 坑 | 现象 | 结论 |
|---|---|---|
| 3D 里直接调手感 | 越调越乱，说不清哪里不对 | 手感参数以 2D 版为准，3D 只做映射 |
| `file://` 加载 GLB | 模型不出现，控制台跨源报错 | 必须起静态服务器 |
| 不加色调映射 | 颜色发灰、像塑料玩具 | ACES + 1.05 曝光 |
| 用 Bloom 做霓虹 | 帧率掉、边缘糊 | 改 CanvasTexture 画发光带 |
| 雾用白色 | 远景像起大雾 | 暖色雾，配合黄昏 |

## 五、还没验证的

- 移动端实测帧率（只在桌面浏览器跑过）
- GLB 减面与贴图压缩的实际收益
- 长时间（>3 分钟）运行的数值漂移，尤其敌车生成间隔收到 0.42 之后
