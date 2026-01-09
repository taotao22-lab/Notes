# Tool Data

## 1. 开源数据集

| 名称 | 数量 | 链接 | 特点 | 备注 |
|-------|-------|-------|-------|-------|
| xlam-function-calling-60k | 60k | [xLAM](https://huggingface.co/datasets/Salesforce/xlam-function-calling-60k) | 单轮 | 最主流，大部分论文都用|
| ToolACE |  | [ToolACE](https://huggingface.co/Team-ACE) | 102154轮，2000多轮 | 第二主流，大部分论文都用|
| Open-Agentic-tool-use| 30k  | [Open-Agentic-tool-use](https://www.modelscope.cn/models/hbg400/Open-Agentic-tool-use) | 多轮
| ToolRet-Training| 20w  | [ToolRet-Training](https://huggingface.co/datasets/mangopy/ToolRet-Training-20w) | 多轮
| DTA-Tool|   | [DTA-Tool](https://huggingface.co/datasets/dongsheng/DTA-Tool) | 
| ToolPref-Pairwise-30K| 30k  | [ToolPref-Pairwise-30K](https://github.com/lirenhao1997/ToolRM) | | 这个数据集挺新的，应该收集了不少数据做后处理
| ToolForge |   | [ToolForge](https://huggingface.co/datasets/buycar/ToolForge_datasets/tree/main) | | 
| ToolBench |   | [ToolBench](https://drive.google.com/drive/folders/1TysbSWYpP8EioFu9xPJtpbJZMLLmwAmL?usp=drive_link) | | 
| ToolMind |   | [ToolMind](https://huggingface.co/datasets/Nanbeige/ToolMind) | | xlam、ToolACE等开源数据 + 部分自己合成的



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
|ToolForge| [ToolForge](https://github.com/Buycar-arb/ToolForge/tree/main) | 多跳搜索的，可以借鉴一下
|ToolGrad| [ToolGrad](https://github.com/zhongyi-zhou/toolgrad) | 先构建工具链，再反向生成query和response


## 5. 数据集样本示例

1. API-Bank中的一个样本

        {
        "system": "You are a helpful multi-turn dialogue assistant capable of leveraging tool calls to solve user tasks and provide structured chat responses.\n\n**Available Tools**\n\nIn your response, you can use the following tools:\n\n1. **Name: QueryHealthData**\n   Description: This API queries the recorded health data in database of a given user and time span.\n   Parameters: {\n     'user_id': {'type': 'str', 'description': 'The user id of the given user. Cases are ignored.'},\n     'start_time': {'type': 'str', 'description': 'The start time of the time span. Format: %Y-%m-%d %H:%M:%S'},\n     'end_time': {'type': 'str', 'description': 'The end time of the time span. Format: %Y-%m-%d %H:%M:%S'}\n   }\n\n2. **Name: CancelRegistration**\n   Description: This API cancels the registration of a patient given appointment ID.\n   Parameters: {\n     'appointment_id': {'type': 'str', 'description': 'The ID of appointment.'}\n   }\n\n3. **Name: ModifyRegistration**\n   Description: This API modifies the registration of a patient given appointment ID.\n   Parameters: {\n     'appointment_id': {'type': 'str', 'description': 'The ID of appointment.'},\n     'new_appointment_date': {'type': 'str', 'description': 'The new appointment date. Format: %Y-%m-%d.'},\n     'new_appointment_doctor': {'type': 'str', 'description': 'The new appointment doctor.'}\n   }\n\n**Steps for Each Turn**\n\n1. **Think:** Recall relevant context and analyze the current user goal.\n2. **Decide on Tool Usage:** If a tool is needed, specify the tool and its parameters.\n3. **Respond Appropriately:** If a response is needed, generate one while maintaining consistency across user queries.\n\n**Output Format**\n\n```plaintext\n<tool_call> Your thoughts and reasoning </tool_call>\n\n<tool_call>\n{\n  \"name\": \"Tool name\",\n  \"parameters\": {\n    \"Parameter name\": \"Parameter content\",\n    \"... ...\": \"... ...\"\n  }\n}\n{\n  \"name\": \"... ...\",\n  \"parameters\": {\n    \"... ...\": \"... ...\",\n    \"... ...\": \"... ...\"\n  }\n}\n...\n</tool_call>\n\n<response> AI's final response </response>\n```\n\n**Important Notes**\n\n1. You must always include the `<tool_call>` field to outline your reasoning. Provide at least one of `</tool_call>` or `<response>`. Decide whether to use `</tool_call>` (possibly multiple times), `<response>`, or both.\n\n2. You can invoke multiple tool calls simultaneously in the `</tool_call>` fields. Each tool call should be a JSON object with a \"name\" field and an \"parameters\" field containing a dictionary of parameters. If no parameters are needed, leave the \"parameters\" field an empty dictionary.\n\n3. Refer to the previous dialogue records in the history, including the user's queries, previous `</tool_call>`, `<response>`, and any tool feedback noted as `<obs>` (if exists).",
        
        "user": "**Dialogue Records History**\n\n<user>\nCan you please modify my appointment scheduled for March 25th with Dr. Kim to March 26th with Dr. Lee?\n</user>\n\n<response>\nSure, I can help you with that. Please provide me with the appointment ID and the new appointment date and doctor's name.\n</response>\n\n<user>\nThe appointment ID is 34567890 and the new date is March 26th with Dr. Lee.\n</user>\n\n<response>\nAlright. I'll modify your appointment now.\n</response>\n\n<user>\nBased on our conversation above, please only make one tool call to solve my need.\n</user>\n",
        
        "answer": [
            {
            "name": "ModifyRegistration",
            "parameters": {
                "appointment_id": "34567890",
                "new_appointment_date": "2023-03-26",
                "new_appointment_doctor": "Dr. Lee"
            }
            }
        ],
        
        "other": {
            "file": "ModifyRegistration-QueryHealthData-CancelRegistration-level-2-2.jsonl",
            "id": 0
        }
        }

2. xLAM

        {
            "id": 8,
            "query": "Determine the peak of the sequence [44, 46, 48, 50, 52] and the light travel time for 25 light years.",
            "answers": [
                {
                    "name": "find_peak_element",
                    "arguments": {
                        "nums": [44, 46, 48, 50, 52]
                    }
                },
                {
                    "name": "light_travel_time",
                    "arguments": {
                        "distance_in_light_years": 25
                    }
                }
            ],
            "tools": [
                {
                    "name": "find_peak_element",
                    "description": "Finds a peak element in a list of integers.",
                    "parameters": {
                        "nums": {
                            "description": "The list of integers.",
                            "type": "List[int]"
                        }
                    }
                },
                {
                    "name": "light_travel_time",
                    "description": "Calculate the time taken for light to travel from one celestial body to another.",
                    "parameters": {
                        "distance_in_light_years": {
                            "description": "The distance between the two celestial bodies in light years.",
                            "type": "int",
                            "default": 299792458
                        },
                        "speed_of_light": {
                            "description": "The speed of light in vacuum, in m/s. Default value is 299792458 m/s.",
                            "type": "int",
                            "default": 299792458
                        }
                    }
                }
            ]
        }


3. ToolACE中的样本

        {
            "system": "You are an expert in composing functions. You are given a question and a set of possible functions. \nBased on the question, you will need to make one or more function/tool calls to achieve the purpose. \nIf none of the function can be used, point it out. If the given question lacks the parameters required by the function,\nalso point it out. You should only return the function call in tools call sections.\nHere is a list of functions in JSON format that you can invoke:\n[{\"name\": \"GetCompetitions\", \"description\": \"Retrieve a list of football competitions that are televised in Peru.\", \"parameters\": {\"type\": \"dict\", \"properties\": {}, \"required\": []}, \"required\": null}, {\"name\": \"Get Manager Placeholder Image\", \"description\": \"Retrieve the manager placeholder image in PNG format for a specific sport.\", \"parameters\": {\"type\": \"dict\", \"properties\": {\"sport\": {\"description\": \"The name of the sport for which the manager placeholder image is required.\", \"type\": \"string\"}}, \"required\": [\"sport\"]}, \"required\": null}, {\"name\": \"Search Competition API\", \"description\": \"Search for competitions in the sports domain\", \"parameters\": {\"type\": \"dict\", \"properties\": {\"name\": {\"description\": \"The name of the competition to search for\", \"type\": \"string\"}}, \"required\": [\"name\"]}, \"required\": null}, {\"name\": \"Get Category Image\", \"description\": \"Retrieve a category image for a specific sport or league.\", \"parameters\": {\"type\": \"dict\", \"properties\": {\"slug\": {\"description\": \"Slug of the sport or league, e.g. `france` `champions-league` `intl`.\", \"type\": \"string\", \"default\": \"france\"}}, \"required\": [\"slug\"]}, \"required\": null}]. \nShould you decide to return the function call(s). \nPut it in the format of [func1(params_name=params_value, params_name2=params_value2...), func2(params)]\n\nNO other text MUST be included. \n",
            "conversations": [
            {
                "from": "user",
                "value": "Hey, can you show me the list of football competitions that are currently televised in Peru?"
            },
            {
                "from": "assistant",
                "value": "[GetCompetitions()]"
            },
            {
                "from": "tool",
                "value": "[{\"name\": \"GetCompetitions\", \"results\": {\"competitions\": [{\"id\": 1, \"name\": \"Copa Libertadores\", \"startDate\": \"2023-02-15\", \"endDate\": \"2023-11-20\"}, {\"id\": 2, \"name\": \"Copa Sudamericana\", \"startDate\": \"2023-03-05\", \"endDate\": \"2023-10-25\"}, {\"id\": 3, \"name\": \"Liga 1 Peru\", \"startDate\": \"2023-02-03\", \"endDate\": \"2023-12-05\"}]}}]"
            },
            {
                "from": "assistant",
                "value": "Here are the football competitions that are currently televised in Peru:\n\n1. Competition: Copa Libertadores\n   - Start Date: 2023-02-15\n   - End Date: 2023-11-20\n\n2. Competition: Copa Sudamericana\n   - Start Date: 2023-03-05\n   - End Date: 2023-10-25\n\n3. Competition: Liga 1 Peru\n   - Start Date: 2023-02-03\n   - End Date: 2023-12-05\n\nDo note that these dates represent the duration of the competitions for the given year. Is there anything else you would like to know?"
            },
            {
                "from": "user",
                "value": "How important are manager images in sports marketing and branding?"
            },
            {
                "from": "assistant",
                "value": "Manager images are quite significant in sports marketing and branding for several reasons:\n\n1. **Identity and Representation**: The image of a manager can become synonymous with the team's identity. It represents leadership and can convey the team's ethos and approach to the sport.\n\n2. **Fan Engagement**: Fans often form emotional connections with team managers. Images used in promotional materials can enhance this bond, making fans feel more connected to the team.\n\n3. **Merchandising**: The use of a manager's image on merchandise can boost sales. Fans are likely to purchase products featuring a well-respected or popular manager as a sign of support or admiration.\n\n4. **Media Relations**: Managers often serve as the face of the team in media appearances. Their image is crucial to creating a positive public perception and can influence media coverage and public relations efforts.\n\n5. **Sponsorship and Partnerships**: A manager with a strong public image can attract sponsorships and partnerships. Brands are more likely to collaborate with teams whose managers are well-regarded and have a positive public image.\n\nOverall, manager images are vital assets in sports branding, helping to shape team image and fan loyalty, while also playing a key role in commercial strategies. If you need any more specific information or examples, feel free to ask!"
            },
            {
                "from": "user",
                "value": "I'd like to see this in practice! Can you fetch me a manager placeholder image for football? Let's bring these theories to life with some quality pixels."
            },
            {
                "from": "assistant",
                "value": "[Get Manager Placeholder Image(sport=\"football\")]"
            },
            {
                "from": "tool",
                "value": "[{\"name\": \"Get Manager Placeholder Image\", \"results\": {\"image\": \"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAZAAAAGQAQMAAAAew5lpAAAAA1BMVEX///+nxBvIAAAASElEQVR4nO3BAQ0AAADCoPdPbQ43oAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIB3AABUexBM.jpg\", \"sport_name\": \"football\"}}]"
            },
            {
                "from": "assistant",
                "value": "Here is the manager placeholder image for football:\n\n![Manager Placeholder Image for Football](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAZAAAAGQAQMAAAAew5lpAAAAA1BMVEX///+nxBvIAAAASElEQVR4nO3BAQ0AAADCoPdPbQ43oAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIB3AABUexBM.jpg)\n\nThis image can typically be used in promotional content, social media, and anywhere a generic representation of a football manager is needed, particularly when no specific individual's image is available. This helps maintain the branding focus while keeping a professional look in all communications."
            }
            ]
        }


4. rlla训练集中的样本

        {
        "data_source": "rlla",
        "prompt": [
            {
            "content": "You are a helpful multi-turn dialogue assistant capable of leveraging tool calls to solve user tasks and provide structured chat responses.\n\n**Available Tools**\nIn your response, you can use the following tools:\n1. Name: GetPowerBINews\nDescription: Retrieve news related to Power BI.\nParameters: {\"page\": {\"description\": \"The page number to retrieve.\", \"type\": \"int\", \"default\": \"\"}}\n2. Name: SingleOffer\nDescription: API that retrieves information for a single offer from Avito\nParameters: {\"singleav\": {\"description\": \"The URL of the Avito offer to retrieve information for\", \"type\": \"string\", \"default\": \"https://www.avito.ma/fr/autre_secteur/maisons_et_villas/Villa_OCP_4_faces_sur_550_metre_de_terrain_49107436.htm\"}}\n3. Name: Domain WHOIS Lookup API\nDescription: Retrieves WHOIS information for a given domain name.\nParameters: {\"domain_name\": {\"description\": \"The domain name for which you would like to retrieve WHOIS details.\", \"type\": \"string\", \"default\": \"\"}}\n4. Name: GetNews\nDescription: Retrieve Azure news articles\nParameters: {\"page\": {\"description\": \"The page number of news articles to retrieve\", \"type\": \"int\", \"default\": \"\"}}\n\n**Steps for Each Turn**\n1. **Think:** Recall relevant context and analyze the current user goal.\n2. **Decide on Tool Usage:** If a tool is needed, specify the tool and its parameters.\n3. **Respond Appropriately:** If a response is needed, generate one while maintaining consistency across user queries.\n\n**Output Format**\n```plaintext\n<thinking> Your thoughts and reasoning </thinking>\n<tool_call>\n{\"name\": \"Tool name\", \"parameters\": {\"Parameter name\": \"Parameter content\", \"... ...\": \"... ...\"}}\n{\"name\": \"... ...\", \"parameters\": {\"... ...\": \"... ...\", \"... ...\": \"... ...\"}}\n...\n</tool_call>\n<response> AI's final response </response>\n```\n\n**Important Notes**\n1. You must always include the `<think>` field to outline your reasoning. Provide at least one of `<tool_call>` or `<response>`. Decide whether to use `<tool_call>` (possibly multiple times), `<response>`, or both.\n2. You can invoke multiple tool calls simultaneously in the `<tool_call>` fields. Each tool call should be a JSON object with a \"name\" field and an \"parameters\" field containing a dictionary of parameters. If no parameters are needed, leave the \"parameters\" field an empty dictionary.\n3. Refer to the previous dialogue records in the history, including the user's queries, previous `<tool_call>`, `<response>`, and any tool feedback noted as `<obs>` (if exists).",
            "role": "system"
            },
            {
            "content": "**Dialogue Records History**\n<user> Hey, I'm looking to stay current on Azure services. Could you fetch the latest news on that topic for me? </user>",
            "role": "user"
            }
        ],
        "ability": "math",
        "reward_model": {
            "ground_truth": "<think> I should use the appropriate tool with proper parameters to respond to the user's need. </thinking>\n<tool_call>\n{\"name\": \"GetNews\", \"parameters\": {\"page\": \"1\"}}\n</tool_call>",
            "style": "rule"
        },
        "extra_info": {
            "index": 0,
            "input": "**Dialogue Records History**\n<user> Hey, I'm looking to stay current on Azure services. Could you fetch the latest news on that topic for me? </user>",
            "instruction": "You are a helpful multi-turn dialogue assistant capable of leveraging tool calls to solve user tasks and provide structured chat responses.\n\n**Available Tools**\nIn your response, you can use the following tools:\n1. Name: GetPowerBINews\nDescription: Retrieve news related to Power BI.\nParameters: {\"page\": {\"description\": \"The page number to retrieve.\", \"type\": \"int\", \"default\": \"\"}}\n2. Name: SingleOffer\nDescription: API that retrieves information for a single offer from Avito\nParameters: {\"singleav\": {\"description\": \"The URL of the Avito offer to retrieve information for\", \"type\": \"string\", \"default\": \"https://www.avito.ma/fr/autre_secteur/maisons_et_villas/Villa_OCP_4_faces_sur_550_metre_de_terrain_49107436.htm\"}}\n3. Name: Domain WHOIS Lookup API\nDescription: Retrieves WHOIS information for a given domain name.\nParameters: {\"domain_name\": {\"description\": \"The domain name for which you would like to retrieve WHOIS details.\", \"type\": \"string\", \"default\": \"\"}}\n4. Name: GetNews\nDescription: Retrieve Azure news articles\nParameters: {\"page\": {\"description\": \"The page number of news articles to retrieve\", \"type\": \"int\", \"default\": \"\"}}\n\n**Steps for Each Turn**\n1. **Think:** Recall relevant context and analyze the current user goal.\n2. **Decide on Tool Usage:** If a tool is needed, specify the tool and its parameters.\n3. **Respond Appropriately:** If a response is needed, generate one while maintaining consistency across user queries.\n\n**Output Format**\n```plaintext\n<thinking> Your thoughts and reasoning </thinking>\n<tool_call>\n{\"name\": \"Tool name\", \"parameters\": {\"Parameter name\": \"Parameter content\", \"... ...\": \"... ...\"}}\n{\"name\": \"... ...\", \"parameters\": {\"... ...\": \"... ...\", \"... ...\": \"... ...\"}}\n...\n</tool_call>\n<response> AI's final response </response>\n```\n\n**Important Notes**\n1. You must always include the `<think>` field to outline your reasoning. Provide at least one of `<tool_call>` or `<response>`. Decide whether to use `<tool_call>` (possibly multiple times), `<response>`, or both.\n2. You can invoke multiple tool calls simultaneously in the `<tool_call>` fields. Each tool call should be a JSON object with a \"name\" field and an \"parameters\" field containing a dictionary of parameters. If no parameters are needed, leave the \"parameters\" field an empty dictionary.\n3. Refer to the previous dialogue records in the history, including the user's queries, previous `<tool_call>`, `<response>`, and any tool feedback noted as `<obs>` (if exists).",
            "output": "<think> I should use the appropriate tool with proper parameters to respond to the user's need. </thinking>\n<tool_call>\n{\"name\": \"GetNews\", \"parameters\": {\"page\": \"1\"}}\n</tool_call>",
            "split": "test"
        }
        }