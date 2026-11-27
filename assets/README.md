# 素材

## 清单

| 文件 | 大小 | 说明 | 来源 |
|---|---|---|---|
| `car.glb` | 325 KB | 主用车辆 | 待补（11-19 04:03 拿到，来源未记录） |
| `cars/hypercar.glb` | 332 KB | 超跑 | 待补 |
| `cars/monster_orange.glb` | 409 KB | 大脚车，橙色 | 待补 |
| `cars/fire_red.glb` | 338 KB | 红色跑车 | 待补 |
| `cars/police_classic.glb` | 300 KB | 复古警车 | 待补 |
| `cars/icecream_pink.glb` | 322 KB | 粉色冰淇淋车 | 待补 |
| `cars/agata.glb` | 166 KB | 另一款车 | 待补 |

**来源一栏目前是空的，这是已知欠账。** 当时还用过 CDN 上的 CC0 低多边形车（`Vuena/free-low-poly-toy-cars-cc0` 里的 `muscle_blue.glb`），说明这批本地模型大概率也来自 CC0 或免费素材库——但没记录就不敢写死，等确认后补上，连带补许可协议。

## 规格

- 格式：GLB（二进制 glTF，模型 + 贴图打包在一个文件）
- 体量：166 ～ 409 KB，适合网页直载，不需要 Draco 压缩
- 未做过减面、贴图压缩、LOD（见 `docs/next-steps.md` 三）

## 加载注意

`file://` 下浏览器会拦 GLB 的跨源读取，必须起本地静态服务器：

```bash
cd .. && python3 -m http.server 8080
```

在 `road-racer-3d.html` 里改 `CONFIG.carURL` 即可换车。

## 待办

- [ ] 补齐每个模型的来源与许可
- [ ] 记录三角面数与贴图尺寸，作为减面基线
- [ ] 统一命名规范（现在是风格混搭：颜色系 / 车型系）
