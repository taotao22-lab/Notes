# 数据相关--Tool Learning

## 1. 开源数据集

| 名称 | 数量 | 链接 | 特点 | 备注 |
|-------|-------|-------|-------|-------|
| xlam-function-calling-60k | 60k | [xLAM](https://huggingface.co/datasets/Salesforce/xlam-function-calling-60k) | 单轮 | 主流，多数论文都用|
| Open-Agentic-tool-use| 30k  | [Open-Agentic-tool-use](https://www.modelscope.cn/models/hbg400/Open-Agentic-tool-use) | 多轮
| ToolRet-Training| 20w  | [ToolRet-Training](https://huggingface.co/datasets/mangopy/ToolRet-Training-20w) | 多轮
| DTA-Tool|   | [DTA-Tool](https://huggingface.co/datasets/dongsheng/DTA-Tool) | 
| ToolPref-Pairwise-30K| 30k  | [ToolPref-Pairwise-30K](https://github.com/lirenhao1997/ToolRM) | | 这个数据集挺新的，应该收集了不少数据做后处理
| ToolForge |   | [ToolForge](https://huggingface.co/datasets/buycar/ToolForge_datasets/tree/main) | | 



## 2. Benchmark


| 名称 |  评价指标 | 备注
|-------|-------|-------|
| Berkeley Function Calling Leaderboard (BFCLv3)| 准确率| 最常用，论文中都有该benchmark的结果
| APIBank| |第二常用，基本上论文中也都有
| ACEBench| | 第三常用
| SealTool |
|Tool-Alpaca|
|Nexus Raven|

## 3. 无人问津的Benchmark，可转化为训练数据
| 名称 |  链接 | 备注
|-------|-------|-------|
|Meta Bench| [Meta Bench](https://huggingface.co/datasets/Shengqian12138/Meta-Bench/tree/main) |
|CallNavi| [CallNavi](https://github.com/Etamin/CallNavi) | 729个问题，579个函数
|HammerBench| [HammerBench](https://github.com/MadeAgents/HammerBench) | 
|ToolHop| [ToolHop](https://huggingface.co/datasets/bytedance-research/ToolHop) | 995个问题，3912个函数
|CriticTool| [CriticTool](https://github.com/Shellorley0513/CriticTool) | 





## 4. 工具数据合成的开源代码

| 名称 |  链接 | 备注
|-------|-------|-------|
|ToolForge| [ToolForge](https://github.com/Buycar-arb/ToolForge/tree/main) | 



