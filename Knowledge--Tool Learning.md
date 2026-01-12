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

### 1. API-Bank中的一个样本

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

### 2. xLAM

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


### 3. ToolACE中的样本

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


### 4. rlla训练集中的样本

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


### 5. APIGen-MT中的样本


    "conversations": [
        {
        "from": "human",
        "value": "Hello, I have a request regarding a past flight reservation, 0U4NPP, from PHL to DEN. I was wondering if I could still cancel it due to personal reasons."
        },
        {
        "from": "gpt",
        "value": "To assist you with the cancellation request, I need to verify a few details:\n\n1. Your user ID.\n2. The reason for cancellation (change of plan, airline cancelled flight, or other reasons).\n\nPlease provide this information so I can check the eligibility for cancellation."
        },
        {
        "from": "human",
        "value": "Sure. My user ID is amelia_rossi_1651. The reason for the cancellation request is personal. I understand if it's an unusual request as the flights have already landed, but I thought it was worth checking if there's any flexibility."
        },
        {
        "from": "function_call",
        "value": "{\"name\": \"get_reservation_details\", \"arguments\": {\"reservation_id\": \"0U4NPP\"}}"
        },
        {
        "from": "observation",
        "value": "{\"reservation_id\": \"0U4NPP\", \"user_id\": \"amelia_rossi_1651\", \"origin\": \"PHL\", \"destination\": \"DEN\", \"flight_type\": \"round_trip\", \"cabin\": \"economy\", \"flights\": [{\"origin\": \"PHL\", \"destination\": \"DEN\", \"flight_number\": \"HAT076\", \"date\": \"2024-05-09\", \"price\": 143}, {\"origin\": \"DEN\", \"destination\": \"PHL\", \"flight_number\": \"HAT080\", \"date\": \"2024-05-13\", \"price\": 147}], \"passengers\": [{\"first_name\": \"Amelia\", \"last_name\": \"Rossi\", \"dob\": \"1954-05-19\"}, {\"first_name\": \"Chen\", \"last_name\": \"Johansson\", \"dob\": \"1956-04-23\"}, {\"first_name\": \"Sophia\", \"last_name\": \"Moore\", \"dob\": \"1989-06-12\"}, {\"first_name\": \"Yusuf\", \"last_name\": \"Brown\", \"dob\": \"1986-11-03\"}], \"payment_history\": [{\"payment_id\": \"credit_card_4240750\", \"amount\": 1280}], \"created_at\": \"2024-05-08T22:44:02\", \"total_baggages\": 4, \"nonfree_baggages\": 0, \"insurance\": \"yes\"}"
        },
        {
        "from": "function_call",
        "value": "{\"name\": \"think\", \"arguments\": {\"thought\": \"The reservation 0U4NPP is for a round trip from PHL to DEN, with flights on May 9 and May 13, 2024. The flights have already been completed, and the user is requesting a cancellation for personal reasons. According to the policy, cancellations cannot be processed for flights that have already been flown, regardless of the reason. Therefore, I need to inform the user that this request cannot be fulfilled and offer to transfer them to a human agent if they need further assistance.\"}}"
        },
        {
        "from": "observation",
        "value": ""
        },
        {
        "from": "gpt",
        "value": "Unfortunately, I cannot process a cancellation for flights that have already been completed, regardless of the reason. If you need further assistance or have any other questions, I can transfer you to a human agent. Please let me know how you would like to proceed."
        },
        {
        "from": "human",
        "value": "I understand and appreciate your help. No need to transfer me to a human agent. Have a good day!"
        },
        {
        "from": "gpt",
        "value": "You're welcome! If you have any more questions in the future, feel free to reach out. Have a great day!"
        }
    ],
    "tools": "[{\"name\": \"book_reservation\", \"description\": \"Book a reservation.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"user_id\": {\"type\": \"string\", \"description\": \"The ID of the user to book the reservation, such as 'sara_doe_496'.\"}, \"origin\": {\"type\": \"string\", \"description\": \"The IATA code for the origin city, such as 'SFO'.\"}, \"destination\": {\"type\": \"string\", \"description\": \"The IATA code for the destination city, such as 'JFK'.\"}, \"flight_type\": {\"type\": \"string\", \"enum\": [\"one_way\", \"round_trip\"]}, \"cabin\": {\"type\": \"string\", \"enum\": [\"basic_economy\", \"economy\", \"business\"]}, \"flights\": {\"type\": \"array\", \"description\": \"An array of objects containing details about each piece of flight.\", \"items\": {\"type\": \"object\", \"properties\": {\"flight_number\": {\"type\": \"string\", \"description\": \"Flight number, such as 'HAT001'.\"}, \"date\": {\"type\": \"string\", \"description\": \"The date for the flight in the format 'YYYY-MM-DD', such as '2024-05-01'.\"}}, \"required\": [\"flight_number\", \"date\"]}}, \"passengers\": {\"type\": \"array\", \"description\": \"An array of objects containing details about each passenger.\", \"items\": {\"type\": \"object\", \"properties\": {\"first_name\": {\"type\": \"string\", \"description\": \"The first name of the passenger, such as 'Noah'.\"}, \"last_name\": {\"type\": \"string\", \"description\": \"The last name of the passenger, such as 'Brown'.\"}, \"dob\": {\"type\": \"string\", \"description\": \"The date of birth of the passenger in the format 'YYYY-MM-DD', such as '1990-01-01'.\"}}, \"required\": [\"first_name\", \"last_name\", \"dob\"]}}, \"payment_methods\": {\"type\": \"array\", \"description\": \"An array of objects containing details about each payment method.\", \"items\": {\"type\": \"object\", \"properties\": {\"payment_id\": {\"type\": \"string\", \"description\": \"The payment id stored in user profile, such as 'credit_card_7815826', 'gift_card_7815826', 'certificate_7815826'.\"}, \"amount\": {\"type\": \"number\", \"description\": \"The amount to be paid.\"}}, \"required\": [\"payment_id\", \"amount\"]}}, \"total_baggages\": {\"type\": \"integer\", \"description\": \"The total number of baggage items included in the reservation.\"}, \"nonfree_baggages\": {\"type\": \"integer\", \"description\": \"The number of non-free baggage items included in the reservation.\"}, \"insurance\": {\"type\": \"string\", \"enum\": [\"yes\", \"no\"]}}, \"required\": [\"user_id\", \"origin\", \"destination\", \"flight_type\", \"cabin\", \"flights\", \"passengers\", \"payment_methods\", \"total_baggages\", \"nonfree_baggages\", \"insurance\"]}}, {\"name\": \"calculate\", \"description\": \"Calculate the result of a mathematical expression.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"expression\": {\"type\": \"string\", \"description\": \"The mathematical expression to calculate, such as '2 + 2'. The expression can contain numbers, operators (+, -, *, /), parentheses, and spaces.\"}}, \"required\": [\"expression\"]}}, {\"name\": \"cancel_reservation\", \"description\": \"Cancel the whole reservation.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"reservation_id\": {\"type\": \"string\", \"description\": \"The reservation ID, such as 'ZFA04Y'.\"}}, \"required\": [\"reservation_id\"]}}, {\"name\": \"get_reservation_details\", \"description\": \"Get the details of a reservation.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"reservation_id\": {\"type\": \"string\", \"description\": \"The reservation id, such as '8JX2WO'.\"}}, \"required\": [\"reservation_id\"]}}, {\"name\": \"get_user_details\", \"description\": \"Get the details of an user.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"user_id\": {\"type\": \"string\", \"description\": \"The user id, such as 'sara_doe_496'.\"}}, \"required\": [\"user_id\"]}}, {\"name\": \"list_all_airports\", \"description\": \"List all airports and their cities.\", \"parameters\": {\"type\": \"object\", \"properties\": {}, \"required\": []}}, {\"name\": \"search_direct_flight\", \"description\": \"Search direct flights between two cities on a specific date.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"origin\": {\"type\": \"string\", \"description\": \"The origin city airport in three letters, such as 'JFK'.\"}, \"destination\": {\"type\": \"string\", \"description\": \"The destination city airport in three letters, such as 'LAX'.\"}, \"date\": {\"type\": \"string\", \"description\": \"The date of the flight in the format 'YYYY-MM-DD', such as '2024-01-01'.\"}}, \"required\": [\"origin\", \"destination\", \"date\"]}}, {\"name\": \"search_onestop_flight\", \"description\": \"Search direct flights between two cities on a specific date.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"origin\": {\"type\": \"string\", \"description\": \"The origin city airport in three letters, such as 'JFK'.\"}, \"destination\": {\"type\": \"string\", \"description\": \"The destination city airport in three letters, such as 'LAX'.\"}, \"date\": {\"type\": \"string\", \"description\": \"The date of the flight in the format 'YYYY-MM-DD', such as '2024-05-01'.\"}}, \"required\": [\"origin\", \"destination\", \"date\"]}}, {\"name\": \"send_certificate\", \"description\": \"Send a certificate to a user. Be careful!\", \"parameters\": {\"type\": \"object\", \"properties\": {\"user_id\": {\"type\": \"string\", \"description\": \"The ID of the user to book the reservation, such as 'sara_doe_496'.\"}, \"amount\": {\"type\": \"number\", \"description\": \"Certificate amount to send.\"}}, \"required\": [\"user_id\", \"amount\"]}}, {\"name\": \"think\", \"description\": \"Use this tool to think and perform reasoning before calling an API. It will not obtain new information or change the database, but just append the thought to the log.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"thought\": {\"type\": \"string\", \"description\": \"The step by step thought process.\"}}, \"required\": [\"thought\"]}}, {\"name\": \"transfer_to_human_agents\", \"description\": \"Transfer the user to a human agent, with a summary of the user's issue. Only transfer if the user explicitly asks for a human agent, or if the user's issue cannot be resolved by the agent with the available tools.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"summary\": {\"type\": \"string\", \"description\": \"A summary of the user's issue.\"}}, \"required\": [\"summary\"]}}, {\"name\": \"update_reservation_baggages\", \"description\": \"Update the baggage information of a reservation.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"reservation_id\": {\"type\": \"string\", \"description\": \"The reservation ID, such as 'ZFA04Y'.\"}, \"total_baggages\": {\"type\": \"integer\", \"description\": \"The updated total number of baggage items included in the reservation.\"}, \"nonfree_baggages\": {\"type\": \"integer\", \"description\": \"The updated number of non-free baggage items included in the reservation.\"}, \"payment_id\": {\"type\": \"string\", \"description\": \"The payment id stored in user profile, such as 'credit_card_7815826', 'gift_card_7815826', 'certificate_7815826'.\"}}, \"required\": [\"reservation_id\", \"total_baggages\", \"nonfree_baggages\", \"payment_id\"]}}, {\"name\": \"update_reservation_flights\", \"description\": \"Update the flight information of a reservation.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"reservation_id\": {\"type\": \"string\", \"description\": \"The reservation ID, such as 'ZFA04Y'.\"}, \"cabin\": {\"type\": \"string\", \"enum\": [\"basic_economy\", \"economy\", \"business\"]}, \"flights\": {\"type\": \"array\", \"description\": \"An array of objects containing details about each piece of flight in the ENTIRE new reservation. Even if the a flight segment is not changed, it should still be included in the array.\", \"items\": {\"type\": \"object\", \"properties\": {\"flight_number\": {\"type\": \"string\", \"description\": \"Flight number, such as 'HAT001'.\"}, \"date\": {\"type\": \"string\", \"description\": \"The date for the flight in the format 'YYYY-MM-DD', such as '2024-05-01'.\"}}, \"required\": [\"flight_number\", \"date\"]}}, \"payment_id\": {\"type\": \"string\", \"description\": \"The payment id stored in user profile, such as 'credit_card_7815826', 'gift_card_7815826', 'certificate_7815826'.\"}}, \"required\": [\"reservation_id\", \"cabin\", \"flights\", \"payment_id\"]}}, {\"name\": \"update_reservation_passengers\", \"description\": \"Update the passenger information of a reservation.\", \"parameters\": {\"type\": \"object\", \"properties\": {\"reservation_id\": {\"type\": \"string\", \"description\": \"The reservation ID, such as 'ZFA04Y'.\"}, \"passengers\": {\"type\": \"array\", \"description\": \"An array of objects containing details about each passenger.\", \"items\": {\"type\": \"object\", \"properties\": {\"first_name\": {\"type\": \"string\", \"description\": \"The first name of the passenger, such as 'Noah'.\"}, \"last_name\": {\"type\": \"string\", \"description\": \"The last name of the passenger, such as 'Brown'.\"}, \"dob\": {\"type\": \"string\", \"description\": \"The date of birth of the passenger in the format 'YYYY-MM-DD', such as '1990-01-01'.\"}}, \"required\": [\"first_name\", \"last_name\", \"dob\"]}}}, \"required\": [\"reservation_id\", \"passengers\"]}}]",
    "system": "# Airline Agent Policy\nThe current time is 2024-05-15 15:00:00 EST.\n\nAs an airline agent, you can help users book, modify, or cancel flight reservations.\n- Before taking any actions that update the booking database (booking, modifying flights, editing baggage, upgrading cabin class, or updating passenger information), you must list the action details and obtain explicit user confirmation (yes) to proceed.\n- You should not provide any information, knowledge, or procedures not provided by the user or available tools, or give subjective recommendations or comments.\n- You should only make one tool call at a time, and if you make a tool call, you should not respond to the user simultaneously. If you respond to the user, you should not make a tool call at the same time.\n- You should deny user requests that are against this policy.\n- You should transfer the user to a human agent if and only if the request cannot be handled within the scope of your actions.\n\n## Domain Basic\n- Each user has a profile containing user id, email, addresses, date of birth, payment methods, reservation numbers, and membership tier.\n- Each reservation has an reservation id, user id, trip type (one way, round trip), flights, passengers, payment methods, created time, baggages, and travel insurance information.\n- Each flight has a flight number, an origin, destination, scheduled departure and arrival time (local time), and for each date:\n    - If the status is \"available\", the flight has not taken off, available seats and prices are listed.\n    - If the status is \"delayed\" or \"on time\", the flight has not taken off, cannot be booked.\n    - If the status is \"flying\", the flight has taken off but not landed, cannot be booked.\n\n## Book flight\n- The agent must first obtain the user id, then ask for the trip type, origin, destination.\n- Passengers: Each reservation can have at most five passengers. The agent needs to collect the first name, last name, and date of birth for each passenger. All passengers must fly the same flights in the same cabin.\n- Payment: each reservation can use at most one travel certificate, at most one credit card, and at most three gift cards. The remaining amount of a travel certificate is not refundable. All payment methods must already be in user profile for safety reasons.\n- Checked bag allowance: If the booking user is a regular member, 0 free checked bag for each basic economy passenger, 1 free checked bag for each economy passenger, and 2 free checked bags for each business passenger. If the booking user is a silver member, 1 free checked bag for each basic economy passenger, 2 free checked bag for each economy passenger, and 3 free checked bags for each business passenger. If the booking user is a gold member, 2 free checked bag for each basic economy passenger, 3 free checked bag for each economy passenger, and 3 free checked bags for each business passenger. Each extra baggage is 50 dollars.\n- Travel insurance: the agent should ask if the user wants to buy the travel insurance, which is 30 dollars per passenger and enables full refund if the user needs to cancel the flight given health or weather reasons.\n\n## Modify flight\n- The agent must first obtain the user id and the reservation id.\n- Change flights: Basic economy flights cannot be modified. Other reservations can be modified without changing the origin, destination, and trip type. Some flight segments can be kept, but their prices will not be updated based on the current price. The API does not check these for the agent, so the agent must make sure the rules apply before calling the API!\n- Change cabin: all reservations, including basic economy, can change cabin without changing the flights. Cabin changes require the user to pay for the difference between their current cabin and the new cabin class. Cabin class must be the same across all the flights in the same reservation; changing cabin for just one flight segment is not possible.\n- Change baggage and insurance: The user can add but not remove checked bags. The user cannot add insurance after initial booking.\n- Change passengers: The user can modify passengers but cannot modify the number of passengers. This is something that even a human agent cannot assist with.\n- Payment: If the flights are changed, the user needs to provide one gift card or credit card for payment or refund method. The agent should ask for the payment or refund method instead.\n\n## Cancel flight\n- The agent must first obtain the user id, the reservation id, and the reason for cancellation (change of plan, airline cancelled flight, or other reasons)\n- All reservations can be cancelled within 24 hours of booking, or if the airline cancelled the flight. Otherwise, basic economy or economy flights can be cancelled only if travel insurance is bought and the condition is met, and business flights can always be cancelled. The rules are strict regardless of the membership status. The API does not check these for the agent, so the agent must make sure the rules apply before calling the API!\n- The agent can only cancel the whole trip that is not flown. If any of the segments are already used, the agent cannot help and transfer is needed.\n- The refund will go to original payment methods in 5 to 7 business days.\n\n## Refund\n- If the user is silver/gold member or has travel insurance or flies business, and complains about cancelled flights in a reservation, the agent can offer a certificate as a gesture after confirming the facts, with the amount being $100 times the number of passengers.\n- If the user is silver/gold member or has travel insurance or flies business, and complains about delayed flights in a reservation and wants to change or cancel the reservation, the agent can offer a certificate as a gesture after confirming the facts and changing or cancelling the reservation, with the amount being $50 times the number of passengers.\n- Do not proactively offer these unless the user complains about the situation and explicitly asks for some compensation. Do not compensate if the user is regular member and has no travel insurance and flies (basic) economy.\n"
