# 六爻纳甲 Skill（liuyao-najia）

传统六爻纳甲占卜（京房八宫体系）的 opencode 功能型技能：**脚本先把卦算准，再由 AI 按方法论解读**。

- GitHub 仓库：https://github.com/stFloat/liuyao-skill
- 排盘引擎移植自 [baiyanwu/liuyao-skill](https://github.com/baiyanwu/liuyao-skill)（MIT），干支与节气由 [sxtwl（寿星天文历）](https://github.com/yuangu/sxtwl_cpp) 精确计算。

- 排盘引擎：`scripts/liuyao.py`（移植自 [baiyanwu/liuyao-skill](https://github.com/baiyanwu/liuyao-skill)，MIT License）。干支与节气由 [sxtwl（寿星天文历）](https://github.com/yuangu/sxtwl_cpp) 精确计算，需先 `pip install sxtwl`。
- 断卦方法论（三份配套文档，详见下方「方法论文档」）：
  - `references/liuyao_method.md` — **操作指南（主）**：取用神→旺衰→动静生克→世应→六神→趋势化断语，含 §3.1 刑冲合害铁律
  - `references/liuyao_basis.md` — **基础讲义**：阴阳/八卦/五行/六亲/用神/动静/进退/月日/暗动/化废/伏反吟理论底稿
  - `references/liuyao_xinghui.md` — **刑合会象义字典**：三刑/六合/三合/三会取象（六合已校正：子丑/寅亥/卯戌/辰酉/巳申/午未）
- 适配协议：Agent Skills（`SKILL.md` 含 `name` + `description`，opencode 自动发现）

## 功能

完整传统六爻流程，全部由脚本确定性计算：

1. **起卦** — 三枚铜钱法（手动掷钱 / 给定六爻值 / 随机生成）
2. **纳甲** — 内外卦分别配天干地支，严格遵循京房口诀（内外卦地支起点不同）
3. **装六亲** — 以宫五行为基准的五行生克关系
4. **安六神** — 按占卦日干起例（青龙朱雀勾陈螣蛇白虎玄武）
5. **定世应** — 八宫体系（本宫→一世→…→五世→游魂→归魂）
6. **查空亡** — 旬空自动推算
7. **查月破** — 月建冲爻判断
8. **变卦** — 动爻变出之卦的完整装卦

## 安装

1. 安装干支/节气计算依赖（排盘精确性的关键）：
   ```bash
   pip install sxtwl
   ```
2. 获取技能（二选一）：
   - **方式 A：克隆到 opencode 的 skills 目录**
     ```bash
     git clone https://github.com/stFloat/liuyao-skill.git ~/.opencode/skills/liuyao-najia
     ```
     （opencode 扫描 `.opencode/skills/**/SKILL.md`，目录名可任意）
   - **方式 B：手动把本目录整体复制到 opencode 的 skills 目录**

```
你的项目/.opencode/skills/liuyao-najia/
├── SKILL.md
├── scripts/liuyao.py
├── references/
│   ├── liuyao_method.md      # 操作指南（主）
│   ├── liuyao_basis.md       # 基础讲义
│   └── liuyao_xinghui.md     # 刑合会象义字典
├── README.md
└── LICENSE
```

opencode 启动时会自动扫描 `.opencode/skills/**/SKILL.md` 并注册本技能。

## 方法论文档

断卦不靠模型临场发挥，全部依据 `references/` 下三份文档，按固定顺序解读：

| 文档 | 角色 | 内容 |
|------|------|------|
| `liuyao_method.md` | **操作指南（主）** | 取用神表 → 旺衰（月建/日辰/空亡/月破）→ 动静生克 → 世应 → 六神 → 趋势化断语；含 §3.1 刑冲合害铁律、归墟/云派变爻作用法、应期思路 |
| `liuyao_basis.md` | 基础讲义 | 阴阳、八卦取象、五行天干地支、六亲、用神章、动静/回头生克/动动相连、进退神、月建+日辰量化、暗动、动爻化废、伏吟反吟 |
| `liuyao_xinghui.md` | 象义字典 | 三刑（子卯/寅巳申/丑未戌/辰午酉亥自刑）、六合（子丑/寅亥/卯戌/辰酉/巳申/午未）、三合局、三会方取象 |

> 引擎 `scripts/liuyao.py` 只算排盘（确定性算法），**不计算任何旺衰/状态评分**；所有「读法」只在上述文档中描述。

## 触发

对 AI 说「六爻」「起卦」「摇卦」「测卦」「占卜」「算一卦」「X 能成吗」，或给出三枚硬币结果 / 六个爻值 / 要求随机起卦，即可触发。

## 用法示例

```bash
# 随机起卦，简略排盘（不落盘）
python scripts/liuyao.py -q "测近期换工作" --no-save

# 完整八步推导（学习/核对）
python scripts/liuyao.py -q "测财运" --full --no-save

# 手动输入六爻值（初爻在前, 6/7/8/9）
python scripts/liuyao.py -q "测感情" -c 7 8 9 6 7 8 --no-save

# 指定占卦日期（影响日辰/空亡/月建/六神）
python scripts/liuyao.py -q "测事业" --date 2026-04-18 --no-save

# 可复现（固定种子）
python scripts/liuyao.py --seed 42 -q "测试" --no-save

# 结构化输出
python scripts/liuyao.py -q "测投资" --json --no-save
```

在 AI 对话中，技能会先问清「所问何事」并让你选起卦方式，跑脚本算出排盘后，再按 `references/` 下的方法论（以 `liuyao_method.md` 为主，辅以 `liuyao_basis.md` 理论底稿与 `liuyao_xinghui.md` 象义字典）给**带依据的趋势化解读**。

### 爻值说明

| 值 | 名称 | 含义 |
|----|------|------|
| 7 | 少阳 | 静阳爻 |
| 8 | 少阴 | 静阴爻 |
| 9 | 老阳 | 阳动爻（阳→阴变） |
| 6 | 老阴 | 阴动爻（阴→阳变） |

## 已知简化

- 月建现已按**节气**由 sxtwl 精确计算（立春→寅月、惊蛰→卯月……），日干支亦由 sxtwl 精确推算，不再使用近似基准。节气交接时刻前后的边界按日期近似处理。
- 排盘为确定性算法；**解读为倾向性、概率性分析，不构成任何预言或保证**。

## 免责声明

- 本工具用于**传统文化研究、学习与交流，以及娱乐性趋势参考**，不构成任何投资、理财、医疗、法律、婚姻或人生决策建议。
- 六爻起卦带有随机性与主观解读成分，结果**仅供参考**，请勿作为唯一依据，更勿以此进行高风险决策（如重仓投资、放弃正规医疗等）。
- 排盘为确定性算法（干支、六亲、世应等由脚本严格按规则计算）；**解读仅为倾向性、概率性分析，不构成任何预言或保证**。
- 凡涉及钱财、健康、法律等重大事项，请以专业机构与持证人士的意见为准。
- 使用本 skill 即表示你理解并接受：一切后果由使用者自行承担，作者与贡献者不对任何直接或间接损失承担责任。

## License

- 本 `liuyao` skill 改编自 [baiyanwu/liuyao-skill](https://github.com/baiyanwu/liuyao-skill)，原项目采用 **MIT 协议**。
- 本仓库同样以 **MIT 协议**开源：你可以自由使用、修改、分发，但须保留原作者署名与许可证声明；软件按「现状」提供，不附带任何担保。
- 许可证全文见仓库内的 `LICENSE` 文件。
