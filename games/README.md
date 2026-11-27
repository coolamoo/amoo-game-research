# 原型一览

三个都是单文件 HTML，不依赖构建工具，不需要 npm。

| 文件 | 画布 / 技术 | 打开方式 | 适合改什么 |
|---|---|---|---|
| `pixel-racer.html` | 320×560 竖屏，纯 Canvas 2D，零依赖 | 双击即可 | 手感、漂移判定、氮气节奏、像素配色 |
| `neon-racer-3d.html` | Three.js 0.149（`three.min.js` 全局构建） | 双击即可 | 霓虹观感、赛道形状、相机跟随 |
| `road-racer-3d.html` | Three.js 0.149（ES Module） | **需起本地服务器** | 车辆物理量纲、色调映射、雾、交通车 |

## 起本地服务器（3D 版加载 GLB 必须）

浏览器在 `file://` 下会拦跨源读取，模型不出现、控制台报错。

```bash
cd ..            # 回到仓库根目录
python3 -m http.server 8080
# 浏览器打开 http://localhost:8080/games/road-racer-3d.html
```

## 操作

键盘 + 指针都支持（`keydown` / `pointerdown`）。方向键或 WASD 控制，具体键位见各文件内注释。触控只是接到同一套输入，没有做移动端手感调校。

## 改造建议

- 想试新玩法：**先在 `pixel-racer` 里做**，2D 迭代快，验证成立再搬到 3D。
- 想换车模：改 `road-racer-3d.html` 里 `CONFIG.carURL`，指向 `assets/cars/*.glb`。
- 改完跑一遍，确认没崩，再把结论写回 `docs/tech-notes.md`。
