# 六爻纳甲 Skill（liuyao.skill · 精简分发版）

传统六爻纳甲占卜（京房八宫体系）的 opencode 功能型技能：**脚本先把卦算准，再由 AI 按方法论解读**。

本仓库是「归墟六爻」项目的**精简分发版**：只含排盘引擎与基础讲义（一份），完整方法论（操作指南、刑合会象义、财运专项）请见完整版仓库 [guixuliuyao.skill](https://gitee.com/dong_wang_han/guixuliuyao.skill.git)。

## 关于本项目的来源与署名

- 排盘引擎 `scripts/liuyao.py` **改编自开源项目 [baiyanwu/liuyao-skill](https://github.com/baiyanwu/liuyao-skill)（MIT 协议）**，原作者署名与 MIT 许可保留于仓库 `LICENSE` 文件中，依 MIT 协议要求不得删除。
- 方法论文档与 README 由 opencode（AI 助手）在作者（归墟六爻项目发起人）的思路指引与提供的讲义、案例素材下撰写整理；**核心想法、占例与判定口径归作者所有**。
- 六爻纳甲本身为公开流传的传统术数体系，本项目的价值在于将排盘与解读**工程化、文档化、可由 AI 稳定复现**。

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

1. 安装干支/节气计算依赖：
   ```bash
   pip install sxtwl
   ```
2. 获取技能（克隆到 opencode 的 skills 目录，目录名任意）：
   ```bash
   git clone https://gitee.com/dong_wang_han/liuyao.skill.git ~/.opencode/skills/liuyao-najia
   ```
   （GitHub 镜像：https://github.com/stFloat/liuyao-skill.git）
   opencode 启动时会扫描 `.opencode/skills/**/SKILL.md` 并自动注册本技能。

## 方法论文档（本仓库所含）

| 文档 | 角色 | 内容 |
|------|------|------|
| `liuyao_basis.md` | 基础讲义 | 阴阳、八卦取象、五行天干地支、六亲、用神、动静/回头生克、进退神、月日量化、暗动、化废、伏吟反吟 |

> 完整方法论（操作指南 `liuyao_method.md`、刑合会象义 `liuyao_xinghui.md`、财运专项 `liuyao_caiyun.md`）见完整版 [guixuliuyao.skill](https://gitee.com/dong_wang_han/guixuliuyao.skill.git)。

> 引擎 `scripts/liuyao.py` 只算排盘（确定性算法），**不计算任何旺衰/状态评分**；所有「读法」在方法论文档中描述。

## 触发

对 AI 说「六爻」「起卦」「摇卦」「测卦」「占卜」「算一卦」「X 能成吗」，或给出三枚硬币结果 / 六个爻值 / 要求随机起卦，即可触发。

## 用法示例

```bash
python scripts/liuyao.py -q "测近期换工作" --no-save
python scripts/liuyao.py -q "测财运" --full --no-save
python scripts/liuyao.py -q "测感情" -c 7 8 9 6 7 8 --no-save
python scripts/liuyao.py -q "测事业" --date 2026-04-18 --no-save
python scripts/liuyao.py --seed 42 -q "测试" --no-save
python scripts/liuyao.py -q "测投资" --json --no-save
```

在 AI 对话中，技能会先问清「所问何事」并让你选起卦方式，跑脚本算出排盘后，再按方法论文档给**带依据的趋势化解读**。

### 爻值说明

| 值 | 名称 | 含义 |
|----|------|------|
| 7 | 少阳 | 静阳爻 |
| 8 | 少阴 | 静阴爻 |
| 9 | 老阳 | 阳动爻（阳→阴变） |
| 6 | 老阴 | 阴动爻（阴→阳变） |

## 已知简化

- 月建按**节气**由 sxtwl 精确计算，日干支亦由 sxtwl 精确推算。节气交接时刻前后的边界按日期近似处理。
- 排盘为确定性算法；**解读为倾向性、概率性分析，不构成任何预言或保证**。

## 免责声明

- 本工具用于**传统文化研究、学习与交流，以及娱乐性趋势参考**，不构成任何投资、理财、医疗、法律、婚姻或人生决策建议。
- 六爻起卦带有随机性与主观解读成分，结果**仅供参考**，请勿作为唯一依据，更勿以此进行高风险决策（如重仓投资、放弃正规医疗等）。
- 排盘为确定性算法；**解读仅为倾向性、概率性分析，不构成任何预言或保证**。
- 凡涉及钱财、健康、法律等重大事项，请以专业机构与持证人士的意见为准。
- 使用本 skill 即表示你理解并接受：一切后果由使用者自行承担，作者与贡献者不对任何直接或间接损失承担责任。

## License

- 排盘引擎 `scripts/liuyao.py` 改编自 [baiyanwu/liuyao-skill](https://github.com/baiyanwu/liuyao-skill)，原项目采用 **MIT 协议**；本仓库依 MIT 协议开源，保留原作者署名与许可证声明，软件按「现状」提供，不附带任何担保。
- 许可证全文见仓库内的 `LICENSE` 文件。
