# DSH 自建技能索引

本机 DeepSeek Harness（DSH）用到的自建技能，分散在几个仓库里，这里统一列个清单方便查找。

共 **4 个仓库 / 4 个技能**（截至 2026-11-22）。点链接直接进仓库，安装命令可直接复制。

## 一览

| 技能 | 用途 | 仓库 | 可见性 | 语言 |
|---|---|---|---|---|
| **stock-factpack** | 股票事实包 + 三段式研判 | [dsh-skills](https://github.com/tel9980/dsh-skills) | 私有 | Python |
| **github-to-cloudflare** | 把 GitHub 仓库部署到 Cloudflare | [github-to-cloudflare-skill](https://github.com/tel9980/github-to-cloudflare-skill) | 公开 | Python · MIT |
| **tcm-herb-formula** | 中药/方剂一站式速查 + 配伍检查 | [tcm-herb-formula-skill](https://github.com/tel9980/tcm-herb-formula-skill) | 公开 | Python |
| **fupan-publish** | A股复盘稿成稿与发布流水线 | [fupan-publish-skill](https://github.com/tel9980/fupan-publish-skill) | 公开 | Python |

---

## 1. stock-factpack · 股票事实包

**仓库**：<https://github.com/tel9980/dsh-skills>（私有 monorepo，技能在 `skills/stock-factpack/`）

微信里发一个 6 位股票代码，DSH 跑 `factpack.py` 取事实包，按「处在什么阶段 / 下一个节点 / 什么情况算看错」三段回答。不给买卖建议、不预测点位。

```bash
git clone https://github.com/tel9980/dsh-skills.git && cd dsh-skills && ./install.sh
# 或单跑：python3 skills/stock-factpack/scripts/factpack.py 300026
```

数据来自自建只读管线（signal-data + 标签/排雷集）。仓库里还带 `AGENTS.md` 全局指令与 `ops/` 备份脚本。

## 2. github-to-cloudflare · 部署到 Cloudflare

**仓库**：<https://github.com/tel9980/github-to-cloudflare-skill>（公开，MIT，派生自 `hxx0611/github-to-cloudflare`）

给一个 GitHub URL 或本地路径，自动判型（Pages / Workers+Assets / Actions / 直传）→ 选通道 → 部署 → 验证 → 记账。含框架对照表与 2026 踩坑清单。

```bash
git clone https://github.com/tel9980/github-to-cloudflare-skill.git ~/.dsh/skills/github-to-cloudflare
```

## 3. tcm-herb-formula · 中药与方剂速查

**仓库**：<https://github.com/tel9980/tcm-herb-formula-skill>（公开）

查单味药性味归经/用量/禁忌/妊娠、按功效反查药、查经典名方君臣佐使与方解、解析处方、十八反十九畏配伍检查，并检索《精选千家妙方》1072 首当代验方。数据来自本草典（CC BY-SA 4.0）、TCM-Prescription-Analyzer（MIT）与《精选千家妙方》，脚本零依赖。**仅作文献检索，不辨证开方。**

```bash
git clone https://github.com/tel9980/tcm-herb-formula-skill.git ~/.dsh/skills/tcm-herb-formula
# 统一入口：python3 ~/.dsh/skills/tcm-herb-formula/scripts/tcm.py --help
```

## 4. fupan-publish · 复盘稿成稿与发布

**仓库**：<https://github.com/tel9980/fupan-publish-skill>（公开）

主力平台是今日头条微头条：收料 → 定题 → 写稿 → 标点/硬检查 → 微头条版 + 一键复制粘贴页 → 纯文字长文 → 台账登记。半角标点自动转全角、内容硬检查五组、≤400 字微头条导出。

```bash
git clone https://github.com/tel9980/fupan-publish-skill.git ~/.dsh/skills/fupan-publish
# 体检+导出：python3 ~/.dsh/skills/fupan-publish/scripts/preflight.py <稿件.md> --fix-punct --export
```

---

## 通用安装说明

DSH 技能目录是 `~/.dsh/skills/<技能名>/`，把仓库 clone 到对应目录即可，装完**不用重启 DSH**，下次对话自动出现在技能目录。

- stock-factpack 在私有 monorepo 里，用仓库自带的 `install.sh`（会同时装全局指令与技能）。
- 其余三个是独立仓库，clone 到 `~/.dsh/skills/` 下对应名字即可。
- 所有脚本都是 **Python 3 + 标准库，零第三方依赖**。

## 维护

- 新增技能：在对应仓库开发 → 推到 GitHub → 回到本索引加一行。
- 更新索引：直接改本 README 后 push。
- 本索引只放链接和说明，不放技能正文。

## 免责

各技能的数据边界与安全规则写在各自 `SKILL.md` 里。金融类只陈述公开数据、不构成投资建议；中药类只作文献检索、不辨证开方、不给用药剂量。
