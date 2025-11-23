# ref 
- sharedAIlab: https://github.com/shareAI-lab/analysis_claude_code/

- claude code 提示词
  - https://gist.github.com/wong2/e0f34aac66caf890a332f7b6f9e2ba8f
  - 


# 开发过程
- 1. 在repoagent的基础上，LLM的消息里 content不再是只有tool和str二选一，单独一个tool_call的字段
- 2. 

# 实现细节

## claude code 的细节
- read的时候 read工具会添加其所在的行号
- search 的时候， 会自动显示其所在的文件行号的命中处，文件大小 和文件行数
- 单独为每个tool的描述构建descriptiond的prompt 
- Claude 的tool 
- LLM支持prompt的tool调用 ， ToolXMLTag, ToolArgsXmlTag ，直接基于正则进行解析 
- 中断处理，一旦用户session 回车文本，或ctrl c, agent stop 函数 对应到 llm 的stop 函数
- 任务管理模块 ： todo tool 模块最终会映射到一个专属于 Claude的工作空间，默认直接是内存模式和在当前的文件夹下
- tool的权限检查允许通配符匹配
- claude code 的执行过程    
  - 1. topic_judge 新的topic直接clear message 
  - 2. build-system-prompt 
  - 3. 

## 扩展细节
-  对于超长步骤任务来说，尤其是coding task 任务来说，需要保存大量的中间结果 , 我觉得需要写入agent的note区域
但是暂时可以不实现，观察执行效果，对于这部分两个考虑 ，是否需要写入，是否需常驻system_prompt（Claude 没有这个tool调用）
- 
  

- 


  
# 架构设计



- Session
  - CliSession  
    - init(cwd,)
    - detect(): 检查命令config ， skill，subagent的配置
    - request_user_confirm: agent请求 命令的执行
    - diplay_event()   
    - log_file_path 
  - Commands
    - commandline
    CommandManger
        - 


-   Agent
    - functioncallAgent
    - ContextAgent
    - ClaudeAgent
      - __init__()
      - building_system_prompt : 拼凑所有的agent 
      - 
-  LLM
   -  openaiLLM
   -  TextLLM
   -  JsonParser
   -  XMLParser
   -  
-  Core
   -  tool_type
   -  agent-type 
   -  message
   -  events
   - 
- Memory 模块
  -  longterm Memory
    -  repo memory
    -  user_memory
    -  project_memory
  -  

- Prompt
  - task_prompt
  - Agent_prompt
  - 
- Tool
  - tool_manger.py
    - tool_right_checking
    - 
  - TopicTool
    - exec(message_list[]) -> new_topic,topic_name
  - BashTool
  - GrepTool
  - ...
  - NoteTool( 与 to_do task 不同的)
    - __init__(memory_or_disk)
    - writing
    - loading
    -  

  - TaskTool: 
    - exec(agent_type, llm_messages, tools )
    - add_subagent_config
    - init_subagent()   
    - loading_
    - system_prompt_info()

  - SkillTool
    - loading_skill
    - 
    - exec() // agent里判断，一旦是skill tool 则加到system prompt中 
  

  - TodoTool
    -  staus
    -  

  - CompressTool
    - compress_prompt : 八段式prompt
    - exec(llm, llm_message, system_prompt)


- util
  - console
  - token_count 
- 
