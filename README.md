# 中文

# 爱心酱抽取机 Lovesause Picker)

> 🎰 一款零依赖、单文件（HTML+CSS+JS）的「抽取机 / 老虎机」小工具：支持**多语言**（中文/English/Русский）、**Base64 内嵌资源**（LOGO / BGM / SFX）、**可点击的泡泡特效**、**可复制结果**、以及**性能友好**的视觉下滚转盘动画。
> ✅ **开箱即用**：把 `html` 放在任意静态环境直接打开/部署即可

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

# English

# Lovesause Picker

> 🎰 A zero-dependency, single-file (HTML+CSS+JS) “picker / slot machine” utility: supports **multiple languages** (中文/English/Русский), **Base64-embedded assets** (LOGO / BGM / SFX), **clickable bubble effects**, **copyable results**, and a **performance-friendly** visual downward-scrolling reel animation.
> ✅ **Works out of the box**: just put `html` on any static host and open/deploy it.

---

## Run locally

> This project is a purely static page—no bundling or backend required.

## Features

* 🎛️ **Multilingual UI**: Chinese / English / Russian; switch via the language button in the top-right corner.
* 🔊 **Audio controls**: Background music (BGM) and sound effects (SFX) have independent toggles. Due to **autoplay policies**, the first playback needs a user gesture; handled in code via `userGestureOnce()`. ([Chrome for Developers][5])
* 🫧 **Interactive bubble effects**: Bubbles continuously spawn in the background; clicking a bubble “pops” it and can trigger a sound effect (if enabled).
* 🎞️ **Visual downward-scrolling reels**: Each column uses flip + looping strip to simulate the “scrolling down” effect, with easing to stop smoothly.
* 🧮 **No duplicate picks**: `randPick(N, K)` draws **K unique integers** (1..N) in place (Fisher-Yates style) and displays them in ascending order.
* 📋 **Copy results**: One-click copy to clipboard (requires HTTPS/localhost). ([MDN Web Docs][6])
* 🧩 **Base64 asset injection**: LOGO / BGM / SFX can be inlined as **data: URLs** via the `assets-json` block at the end of the page—no extra files. ([MDN Web Docs][7])
* ♿ **Reduced-motion friendly**: Respects `prefers-reduced-motion` and degrades the fixed background-texture effect on mobile. ([MDN Web Docs][8])

---

## Page structure & key modules

* **Card-style main UI**: Enter total **N** and column count **K**, click “Start”; results are shown live and appended to the **draw history**.
* **Localization pack**: `I18N = { zh, en, ru }`; `applyLang()` maps strings to the DOM.
* **Audio**: Controlled via `<button>`s. Due to browser **autoplay policy** (requires a user gesture first), playback is attempted after the first interaction. ([Chrome for Developers][5])
* **Background texture & LOGO**: CSS `background` layers (halftone + large/small LOGO watermark). `--logo-url` is injected by JS at startup (prefers `assets-json.logo` if present).
* **Reel mechanics**: Each column duplicates its DOM list (1..Ndom, then repeated) to create an “infinite” scrolling effect; it snaps precisely to the target cell at the end.
* **DOM throttling**: For large N, use `Ndom = min(Nraw, MAX_N_DOM(=5000))` to reduce memory and layout pressure.

> The texture layer uses `background-attachment: fixed` on desktop for steadier parallax. It **degrades** automatically on mobile/reduced-motion contexts. See MDN for property semantics. ([MDN Web Docs][9])

---

# Русский

# Lovesause Picker

> 🎰 Небольшой инструмент «лототрон / слот-машина» **без зависимостей**, в **одном файле** (HTML+CSS+JS): поддерживает **многоязычный интерфейс** (中文/English/Русский), **встроенные в Base64 ресурсы** (LOGO / BGM / SFX), **кликабельные пузырьки**, **копирование результата**, а также **дружественную к производительности** анимацию визуального прокручивания барабанов вниз.
> ✅ **Готов из коробки**: поместите `html` в любой статический хостинг и просто откройте/разверните.

---

## Запуск локально

> Проект — полностью статическая страница; сборка и бэкенд **не требуются**.

## Возможности

* 🎛️ **Многоязычный UI**: Chinese / English / Russian; переключение кнопкой языка в правом верхнем углу.
* 🔊 **Управление звуком**: Фоновая музыка (BGM) и звуковые эффекты (SFX) включаются независимо. Из-за **политики автопроигрывания** первый запуск требует жест пользователя; в коде обработано через `userGestureOnce()`. ([Chrome for Developers][5])
* 🫧 **Интерактивные пузырьки**: Пузырьки непрерывно генерируются на фоне; клик по ним «лопает» их и может воспроизводить эффект (если включено).
* 🎞️ **Визуально прокручивающиеся вниз барабаны**: Каждый столбец использует переворот + циклическую ленту для имитации «прокрутки вниз» и плавной остановки с помощью функции сглаживания.
* 🧮 **Выбор без повторов**: `randPick(N, K)` выбирает **K уникальных целых чисел** (1..N) методом перемешивания на месте и выводит их по возрастанию.
* 📋 **Копирование результата**: Одним кликом в буфер обмена (требуется HTTPS/localhost). ([MDN Web Docs][6])
* 🧩 **Встраивание ресурсов Base64**: LOGO / BGM / SFX можно встроить как **data: URL** через блок `assets-json` в конце страницы — без дополнительных файлов. ([MDN Web Docs][7])
* ♿ **Учёт предпочтения снижения анимации**: Поддержка `prefers-reduced-motion`; на мобильных устройствах эффект фиксированной фоновой текстуры автоматически **упрощается**. ([MDN Web Docs][8])

---

## Структура страницы и ключевые модули

* **Основной интерфейс-карточка**: Введите «общее число N» и «число колонок K», нажмите «Начать»; результаты отображаются сразу и добавляются в **историю жеребьёвок**.
* **Пакет локализаций**: `I18N = { zh, en, ru }`; `applyLang()` маппит строки в DOM.
* **Аудио**: Управление через кнопки `<button>`. Из-за **политики автопроигрывания** браузера (первым делом требуется жест пользователя) попытка воспроизведения делается после первой интеракции. ([Chrome for Developers][5])
* **Фоновая текстура и LOGO**: Многослойный CSS-фон (пуантилизм + крупный/малый водяной знак LOGO); `--logo-url` инъецируется JS при старте (приоритет у `assets-json.logo`).
* **Механика барабанов**: Для каждого столбца дублируется DOM-список (1..Ndom, затем повтор) для «бесконечной» прокрутки; в конце точно выравнивается по целевой ячейке.
* **Ограничение DOM**: При больших N используется `Ndom = min(Nraw, MAX_N_DOM(=5000))` для снижения нагрузки на память и раскладку.

> Слой текстуры использует `background-attachment: fixed` на десктопе для более стабильного параллакса; в мобильных и при сниженной анимации происходит автоматическое **упрощение**. Семантика свойства — на MDN. ([MDN Web Docs][9])
