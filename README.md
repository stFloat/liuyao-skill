# 六爻纳甲 Skill（liuyao-najia）

传统六爻纳甲占卜（京房八宫体系）的 opencode 功能型技能：**脚本先把卦算准，再由 AI 按方法论解读**。

- GitHub 仓库：https://github.com/stFloat/liuyao-skill
- 排盘引擎移植自 [baiyanwu/liuyao-skill](https://github.com/baiyanwu/liuyao-skill)（MIT），干支与节气由 [sxtwl（寿星天文历）](https://github.com/yuangu/sxtwl_cpp) 精确计算。

- 排盘引擎：`scripts/liuyao.py`（移植自 [baiyanwu/liuyao-skill](https://github.com/baiyanwu/liuyao-skill)，MIT License）。干支与节气由 [sxtwl（寿星天文历）](https://github.com/yuangu/sxtwl_cpp) 精确计算，需先 `pip install sxtwl`。
- 断卦方法论：`references/liuyao_method.md`（本技能基于《增删卜易》《卜筮正宗》等公版经典自研）
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
├── references/liuyao_method.md
├── README.md
└── LICENSE
```

opencode 启动时会自动扫描 `.opencode/skills/**/SKILL.md` 并注册本技能。

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

在 AI 对话中，技能会先问清「所问何事」并让你选起卦方式，跑脚本算出排盘后，再按 `references/liuyao_method.md` 给**带依据的趋势化解读**。

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

传统命理学的数字化整理与学习工具，供文化研究、自我认知参考之用。不宣扬封建迷信，不承诺改运消灾转运，不提供任何「法术」类建议。凡涉及健康、婚姻、投资、职业等真实人生决策，请以现实为准、结合专业意见理性判断。

## License

排盘脚本 `scripts/liuyao.py` 采用 **MIT License**，版权归 [baiyanwu/liuyao-skill](https://github.com/baiyanwu/liuyao-skill) 原作者所有（见 `LICENSE`）。其余内容（SKILL.md、references、README）基于公版经典整理，可自由用于学习与研究。
