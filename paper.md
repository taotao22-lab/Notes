# 论文笔记--Tool Learning

## 1. <font color="red">ToolRL</font>: Reward is All Tool Learning Needs


- **拟解决的问题**：提高大模型的工具调用能力。

- **采用的方法**：**强化学习**进行后训练的方法，**GRPO**（去掉KL约束项，以更快的收敛）；奖励为**格式奖励**和**正确性奖励**的和，$R_{\text{final}} = R_{\text{format}} + R_{\text{correct}} \in [-3, 4]$。

- **主要观察**：
    
    1. **SFT**相比较**RL**的缺点是更容易**过拟合**
    
    2. 更长的推理（“**多思考**”）看起来是好事，但对于“**工具使用**”这类任务，并**不有益**

    3. 对于RL来说，**reward**是很关键的，论文重点探讨reward的不同设计模式

    4. 在对**格式奖励**和**正确性奖励**进行加权时，**不能突变**，突变让GRPO的训练结果变得更差。

## 2. <font color="red">ToolACE</font>: Winning the Points of LLM Function Calling


- **拟解决的问题**：构造大模型的**函数调用数据集**

- **采用的方法**：1、合成API；2、模拟用户/助手/工具三种角色的真实交互，生成数据；3、过滤，采用规则检查（语法/参数）和模型检查（一致性/幻觉）

- **主要观察**：
    
    1. 对**合成数据的有效使用**可以提高LLM的能力
    
    2. 生成的数据必须与模型的能力相匹配，不能用同一套数据训练所有模型。


## 3. <font color="red">Nemotron-Research-Tool-N1</font>: Exploring Tool-Using Language Models with Reinforced Reasoning


- **拟解决的问题**：提升LLM使用外部工具时的推理能力

- **采用的方法**：强化学习的方法，使用GRPO算法，设计二元奖励函数（**结构和内容完全正确才奖励为1**，否则奖励为0）

- **主要观察**：
    
    1. 把**一个多轮数据**转换为**多个单轮数据**，ToolRL也是这么干的

    2. R1 风格的强化学习**无需精心准备推理轨迹**即可获得推理技能，这使得直接从现有的监督微调数据中训练推理能力成为可能
    
    3. 广泛采用的"先SFT、后RL"范式，在Tool Learning方面，其性能未必优于**直接使用RL**

    4. **格式奖励**不仅仅是一个简单的输出格式检查，它更是一种训练策略。通过强制模型输出结构化的推理过程，引导其从“记忆者”转变为“思考者”，从而获得了处理前所未见的分布外输入的泛化能力（**out of distribution**）；格式检查，就是检查推理过程是否被包含在标签之内，包含\<think>$\cdots$\</think>和\<tool_call>$\cdots$\</tool_call>

    5. R1风格训练方法的scaling law，随模型规模增大而显著提升

    6. no-reason SFT 和 reason-sft 的区别主要在于训练过程中是否强制模型生成结构化推理格式（即是否要求模型在 \<think> 和 \</think> 标签内输出推理步骤）

    7. 对于工具调用类的训练，长的推理链不一定好


## 4. <font color="red">xLAM</font>: A Family of Large Action Models to Empower AI Agent Systems


- **拟解决的问题**：在工具调用数据集和模型方面，缺少开源工作

- **采用的方法**：在**数据处理**方面，采用如下步骤：统一数据格式→数据增强→数据合成→混合训练策略；在**模型训练**方面，采用先SFT（基础能力）、后DPO（输出质量优化）的方法。

- **主要观察**：
    
    1. 采用的数据增强方法：
    
        - 打乱工具列表顺序、参数顺序
        - 使用不同连接标记，如"[START/END OF QUERY]"、"\<query>\</query>"、纯文本
        - 重述任务指令
        - 不同的输出格式（JSON、XML、YAML）


## 5. <font color="red">APIGen</font>: Automated Pipeline for Generating Verifiable and Diverse Function-Calling Datasets


- **拟解决的问题**：现有工作缺少工具调用的训练数据，这篇论文**开源**了**6w条数据**，是xLAM制作数据的方法

- **采用的方法**：构建一个自动化的**数据生成pipeline**，通过多阶段验证确保数据质量。数据生成流程可概括为：

    - **种子数据采样**：从API库中随机采样1个或多个API及其示例QA对，例如query="查询北京天气"，answer="get_weather('北京'，'today')"
    - **标准化格式化**：将种子数据转换为统一JSON格式（含query、answer字段），例如
    
            {
                "query": "查询北京今天的天气", 
                "answer": 
                {
                    "func": "get_weather", 
                    "args": 
                    {
                        "city": "北京", 
                        "date": "today"
                    }
                }
            }

    - **LLM生成新QA对**：根据目标查询风格（如简单/并行调用），选择提示模板引导LLM生成JSON格式的新的函数调用，如
        
            {
                "query": "明天上海温度多少？",
                "answer": 
                    {
                        "func": "get_weather", 
                        "args": 
                        {
                            "city": "上海", 
                            "date": "tomorrow"
                        }
                    }
            }

- **主要观察**：
    
    1. 数据验证过程：
    
        - **格式**：格式检查
        - **执行**：函数执行检查
        - **语义**：用另一个LLM评估执行结果是否真正回答了用户问题

    2. 生成的数据**样本示例**

            {
            "id": 0,
            "query": "Where can I find live giveaways for beta access and games?",
            "answers": [
                {
                "name": "live_giveaways_by_type",
                "arguments": {
                    "type": "beta"
                }
                },
                {
                "name": "live_giveaways_by_type",
                "arguments": {
                    "type": "game"
                }
                }
            ],
            "tools": [
                {
                "name": "live_giveaways_by_type",
                "description": "Retrieve live giveaways from the GamerPower API based on the specified type.",
                "parameters": {
                    "type": {
                    "description": "The type of giveaways to retrieve (e.g., game, loot, beta).",
                    "type": "str",
                    "default": "game"
                    }
                }
                }
            ]
            }


## 6. <font color="red">APIGen-MT</font>: Agentic PIpeline for Multi-Turn Data Generation via Simulated Agent-Human Interplay


- **拟解决的问题**：制作**多轮**工具调用数据集，开源了5k条

- **采用的方法**：一个两阶段框架，通过模拟人机交互生成高质量的多轮代理数据。阶段1生成**任务蓝图**，包含用户质量$q$、真实动作$a_{gt}$、预期输出$o_{gt}$；阶段2基于阶段1的任务蓝图，通过**模拟人工交互**生成真实对话轨迹。

- **主要观察**：
    
    1. 通过**随机游走**来探索不同路径，感觉是生成agent数据的必要步骤
    
    2. 在使用大模型模拟人类时，可能会**随assistant的思路**走，**偏离**了自己**预设的角色和行为**。这种时候，本论文让模拟人类在需要生成回复时，不是只生成一个，而是一次性生成4个回复，并让模拟人类自己（通过一个评判机制）从这4个候选回复中，选出最好的一个（不知道为什么这么做有效果，但不妨先记录下来）

    3. **多轮对话的SFT训练**：收集的每条对话中，在assistant的每一次回复处都切一刀，生成一个训练样本；训练时，用“历史+当前轮问题”作为输入，让模型只学“当前轮回答”部分(只计算assistant本轮回复的loss)


## 7. <font color="red">DiaTool-DPO</font>: Multi-Turn Direct Preference Optimization for Tool-Augmented Large Language Models


- **拟解决的问题**：解决多轮对话中，工具调用所需**参数缺失**问题以及**工具拒绝**问题（用户请求的功能不在工具列表中，模型应拒绝，而非强行调用不匹配工具）

- **采用的方法**：采用多轮直接偏好优化（**DPO**）；定义马尔科夫决策过程，划分5种内部状态（初始、工具选择中、工具选择完成、等待响应、完成）；基于状态转移轨迹将用户查询分为3类（完整工具调用、需槽位填充、拒绝工具调用）；通过对比正确对话流与错误对话流进行学习。

- **主要观察**：
    
    1. 对多轮对话进行加权评分，通过降低后续轮次的权重，突出**早期回答**的**关键性**
    
    2. 对于**槽位填充**任务：坏的轨迹轮次更少，因为模型可能会遗漏需要填写的槽位回答，导致对话提前结束；**好的轨迹轮次更长**，因为模型正确完成了所有必要的问答轮次。对于**相关性判断**任务：好的轨迹在一轮内就完成了，例如模型正确地通过一次工具调用拒绝了不相关的请求；**坏的轨迹轮次更长**，因为模型判断错误，继续进行了不必要或不相关的多轮对话。


    3. 接触**不同难度级别**的数据有助于提升槽位填充能力的训练

    4. 这篇论文没有涉及并行工具调用，参考意义不大


## 8. <font color="red">ToolRM</font>:Outcome Reward Models for Tool-Calling Large Language Models


- **拟解决的问题**：缺少**工具调用场景**下的**奖励模型**工作

- **采用的方法**：生成错误工具调用数据，采用**对比学习**的方法训练专门的结果奖励模型(ToolRM)，并通过Best-of-n采样或数据过滤提升下游工具调用性能

- **主要观察**：
    
    1. **可利用**：这篇论文可以用来对训练数据做**过滤**，比如收集其他已开源的数据，然后用该模型对数据做过滤

    2. 这篇论文训练的奖励模型还挺大的，比如几个B，这么大的奖励模型是否有必要？
    


        