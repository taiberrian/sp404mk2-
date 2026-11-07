# Roland SP-404MKII 交互式速查表

一款基于SvelteKit和TypeScript开发的、适用于**Roland SP-404MKII**（固件版本v5.50）的快速可视化快捷键查找工具与硬件配套辅助工具。

🔗 **在线工具:** [SP-404MKII 交互式速查表](https://cntrvsy.github.io/sp404mk2cheatsheet/)

---

## 功能特性

- **双向硬件高亮:** 点击虚拟面板上的旋钮、打击垫或按钮，筛选匹配的快捷键。点击任意卡片，预览按键组合以及OLED屏幕显示状态。
- **物理肌肉记忆布局:** 在移动端和桌面端保持1:1的硬件几何尺寸，同时提供用于触控区域的拇指缩放开关。
- **跨平台数字音频工作站 / 术语搜索:** 通过 [`src/lib/data/aliases.json`](./src/lib/data/aliases.json)，使用 MPC、Ableton、FL Studio 或者社区术语（"choke group"、"one shot"、"16 levels"、"lazy chop"、"resample bounce"）进行搜索。
- **精选工作室表单及PDF文件:** 将常用快捷指令固定到自定义工作表，并导出一份紧凑、适合打印的1–2页参考表。
- **严格验证:** 每一个版本都对照罗兰官方参考手册核验原文引用，共101条快捷指令。
---

## 验证流程

为消除传闻与固件偏差，所有数据均对照手册进行核验:

- **参考语料库 ([`docs/reference/roland-v550/`](./docs/reference/roland-v550/)):** Roland v5.50 手册章节，已提取并附带页面锚点 (`<!-- [Page N] -->`).
- **闭环 CI 校验器 ([`scripts/validate-tips.mjs`](./scripts/validate-tips.mjs)):** 在执行 `npm run check` 时自动运行，用来保证:
  1. 确保 `buttons`与`combo`中的硬件ID完全匹配。
  2. 每个提示中的`citation.quote` 内容与所引用的手册页上的原文完全一致。
  3. 在 `aliases.json` 中搜索别名时，既能保持 1:1 的覆盖率，又不会导致关键词冗余。
- **PDF 导入 ([`scripts/ingest-pdf.mjs`](./scripts/ingest-pdf.mjs)):** 将新手册解析为包含自动提取的快捷方式的章节 Markdown 文件：
  ```sh
  npm run docs:ingest -- --pdf /path/to/manual.pdf --slug roland-v600 --title "Roland SP-404MKII Manual v6.00"
  ```

---

## Porting to Other Samplers (SP-303, SP-404SX, etc.)

The engine is modular and can be adapted for any hardware device:

1. **Ingest Manual:** Run `npm run docs:ingest -- --pdf <file.pdf> --slug <id> --title "<title>"` to generate anchored chapter markdown.
2. **Define Hardware Types ([`src/lib/types.ts`](./src/lib/types.ts)):** Update `Category`, `CATEGORIES`, and button IDs (e.g. 8 pads for SP-303, 12 for SP-404SX).
3. **Update Validator ([`scripts/validate-tips.mjs`](./scripts/validate-tips.mjs)):** Align `VALID_CATEGORIES` and `comboTokenToButtons()` with your machine's conventions.
4. **Author Tips & Slang:** Populate [`src/lib/data/tips.json`](./src/lib/data/tips.json) with verbatim quotes and map colloquial terms in [`src/lib/data/aliases.json`](./src/lib/data/aliases.json).
5. **Adapt Faceplate ([`src/lib/components/Faceplate.svelte`](./src/lib/components/Faceplate.svelte)):** Adjust the SVG layout and control arrays (`topKnobs`, `fxButtons`, `pads`) to match the hardware's front panel.

---

## Development

```sh
# Install dependencies
npm install

# Start development server
npm run dev

# Run tip citation validator and Svelte type-checking
npm run check

# Check code formatting and ESLint rules
npm run lint

# Build static production site (./build)
npm run build
```

---

## License & Attribution

- Built with ❤️ by **[frstudios](https://frstudios.co.ke)**
- Repository: **[github.com/cntrvsy/sp404mk2cheatsheet](https://github.com/cntrvsy/sp404mk2cheatsheet)**
- Licensed under the **[MIT License](./LICENSE)**
- _Disclaimer:_ Roland and SP-404MKII are registered trademarks of Roland Corporation. This project is an independent educational tool.
