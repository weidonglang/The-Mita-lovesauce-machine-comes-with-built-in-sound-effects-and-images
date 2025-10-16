# 爱心酱抽取机 (Aixin-chan Picker)

> 🎰 一款零依赖、单文件（HTML+CSS+JS）的「抽取机 / 老虎机」小工具：支持**多语言**（中文/English/Русский）、**Base64 内嵌资源**（LOGO / BGM / SFX）、**可点击的泡泡特效**、**可复制结果**、以及**性能友好**的视觉下滚转盘动画。
> ✅ **开箱即用**：把 `index.html` 放在任意静态环境直接打开/部署即可

---
## 本地运行

> 本项目为纯静态页面，无需打包与后端。

## 功能特性

* 🎛️ **多语言 UI**：中文 / English / Русский，点击右上角语言按钮即切换。
* 🔊 **音频控制**：背景音乐（BGM）与音效（SFX）独立开关。受**自动播放策略**影响，首次播放需要用户手势；已在代码中以 `userGestureOnce()` 处理。([Chrome for Developers][5])
* 🫧 **可交互泡泡特效**：背景持续生成泡泡，点击命中可「爆裂」并触发音效（若开启）。
* 🎞️ **视觉下滚转盘**：每列使用翻转+循环条带的方式模拟「向下滚动」视觉，并用缓动函数平滑停止。
* 🧮 **无重复抽取**：`randPick(N, K)` 以原地洗牌方式抽取 **K 个唯一整数**（1..N）并升序展示。
* 📋 **复制结果**：一键复制到剪贴板（需要 HTTPS/localhost）。([MDN Web Docs][6])
* 🧩 **Base64 资源注入**：LOGO / BGM / SFX 可通过页面尾部的 `assets-json` 以 **data: URL** 内联，无需额外文件。([MDN Web Docs][7])
* ♿ **更少动效优先**：对 `prefers-reduced-motion` 与移动端做了背景纹理固定效果的降级处理。([MDN Web Docs][8])

---

## 页面结构与关键模块

* **卡片主界面**：输入「总数 N」与「列数 K」，点击「开始抽取」；结果实时展示，并追加到「抽签历史」。
* **语言包**：`I18N = { zh, en, ru }`；`applyLang()` 负责将文案映射到 DOM。
* **音频**：通过 `<button>` 控制 BGM/SFX。受浏览器**自动播放策略**限制（需用户手势后才能响），因此首次交互后再尝试播放。([Chrome for Developers][5])
* **背景纹理与 LOGO**：使用 CSS `background` 叠加网点+大/小 LOGO 暗纹；`--logo-url` 由 JS 在启动时注入（优先使用 `assets-json.logo`）。
* **转盘机制**：每列两倍 DOM 列表（1..Ndom 再拼一遍）形成「无限」滚动视觉；结束时精确对齐目标单元格。
* **DOM 限流**：大 N 时采用 `Ndom = min(Nraw, MAX_N_DOM(=5000))` 以降低内存与布局压力。

> 纹理层使用 `background-attachment: fixed` 在桌面端获得更稳的视差观感；移动端/减动效场景自动**降级**。属性语义可参考 MDN。([MDN Web Docs][9])

---

## 配置与二次开发

### 1) Base64 资源（LOGO / BGM / SFX）

在页面底部的 `<script id="assets-json" type="application/json">` 中填入（示例）：

```json
{
  "logo": "data:image/png;base64,....",
  "bgm":  "data:audio/mp3;base64,....",
  "pop":  "data:audio/mp3;base64,...."
}
```

* **data: URL** 说明见 MDN；适合小体积静态资源的内嵌。([MDN Web Docs][7])
* CSS 可直接在 `url(...)` 中使用 `data:` 资源。([MDN Web Docs][10])

### 2) CSS 变量（主题与纹理）

在 `:root` 中集中管理，例如：

* `--bg1 / --bg2`：背景渐变主色
* `--cell`：转盘单元格高度（默认 `64px`）
* `--reel-w`：单列宽度（默认 `86px`）
* `--pat-size-lg / --pat-size-sm / --pat-dot / --pat-offset / --pat-opacity`：暗纹格栅与大小、透明度

> 你也可以将 `--logo-url` 指向一张 `data:` 图片（来自上面的 `assets-json.logo`），即可替换纹理暗纹内容。

### 3) 多语言扩展

在 `I18N` 中新增语言键值并在语言按钮区追加 `data-lang="xx"` 的按钮；`applyLang()` 会自动刷新大部分文案。

### 4) 结果格式与历史

* 修改 `addHistory()` 可自定义时间格式与展示顺序；默认**最新在上**。
* 结果展示容器带 `aria-live="polite"`，能在读屏器中温和播报更新。

---

## 可访问性与性能

* **减少动效支持**：使用 `@media (prefers-reduced-motion: reduce)` 时关闭/降级部分视觉效果，避免造成眩晕或不适。([MDN Web Docs][8])
* **背景固定的取舍**：`background-attachment: fixed` 在桌面端表现良好，但在某些移动设备上渲染代价较高，因此在移动端/减动效场景自动降级为非固定。([MDN Web Docs][9])
* **音频自动播放策略**：现代浏览器默认**禁止带声音的自动播放**；需要用户手势（点击等）后才能 `play()` 成功，代码已在第一次交互后尝试恢复。([Chrome for Developers][5])
* **剪贴板权限**：`navigator.clipboard.writeText` 要求**安全上下文（HTTPS/localhost）**并由用户手势触发才最稳。([MDN Web Docs][6])
