
# Problem statement 3 - Agentic AI tools
1. Problem - API are not straightforward. They have domain specific rules like contract to be adhered to like id. Examples, instead of using id:UTS-16, agent uses id:16 and receives runtime error.
    1. Solution - WILDAGTEVAL is novel benchmark consisting of 60 specific scenarios and 32 tests. This benchmark can be used to evlauate the agentic LLM.
    2. Paper - https://arxiv.org/pdf/2601.00268v1
2. Problem - LLMs that are trained in RL are constrainted by human curated data, so data is not huge. Scalability in training bottleneck, time consuming, costly, labour intensive, 
    1. Solution - Self evolving framework that generate their own data. Problem - Again biased or limited by LLM's own knowledge. You dont know what you dont know. Leading to stagnation. 
    2. Paper - https://arxiv.org/pdf/2511.16043v1
3. Problem - LLM as Judge - LLMs used as judge are limited on text based reasoning. Cannot operate on complex data or compute accurately. Example, print("hello world") is right or wrong is based on text based validation. It can be improved by using tools. 
    1. Solution - Use LLM tools. Code execution tool can verify the instruction instead of LLM error-prone text based validation. Problem - Tools are used only in inference time, so the mapping between inference and reasoning is limited. Also the tools are domain specific, so LLM tool use cannot be applied general purpose.
    2. Solution - TIR - Judge (Tool Integrated Reasoning). Framework for training LLM Judges using multi-turn RL.
    3. https://arxiv.org/pdf/2510.23038
4. Problem - With the rise of MCP, the number of tools have increased. Around 18000 MCP server projects in Github. This can be an alternative to web browser and requires minimal GUI maintenance. But current tools require browser for interacting with environment. 
    1. We introduce TheMCPCompany, an extension of TheAgentCompany (Xu et al., 2024a) that simulates a software company where MCP tools are available for all operations in the company.
    2. https://arxiv.org/pdf/2510.19286
5. Problem - Long and complex context does not work with agent 
    1. https://arxiv.org/abs/2501.10132
6. Problem - 
    1. https://arxiv.org/pdf/2510.07768
7. Problem - Number of MCP tools more than 10K, existing MCP benchmarks are limited to singleserver settings with only a few tools, hindering effective evaluation of agent capabilities in large-scale, real-world scenarios. More of how to evaluate tool augumented LLMs on large scale tools? Scalability problem.
    1. Solution - LiveMCPBench. To support a scalable and reproducible evaluation pipeline in large-scale MCP environments, we curate LiveMCPTool, a diverse and readily deployable collection of 70 MCP servers and 527 tools.
8. https://arxiv.org/pdf/2508.01780
9. Problem - The tools augumented LLMs are trained to think more, rather than diagnose the error. So they are fragile in multi turn interaction setting, model trends to repeat the same mistake again.
    1. Solution - Have a structured reflection process. Tool Reflection Bench - TRB.
    2. https://arxiv.org/abs/2509.18847
10. Problem - Generating MCP servers from API is very less. So can it be automated?
    1. Solution - To address this gap, we present AutoMCP, a compiler that generates MCP servers from OpenAPI 2.0/3.0 specifications.
    2. https://www.arxiv.org/pdf/2507.16044
11. Problem - In enterprise setting, although agents show good potential, they perform poorly when required to follow policies involving sophisticated navigation or branching logic according to company guidelines
        1. Solution - Appending company policies to prompts. Problem - But it is not scalable, and is very crucial to get right. Make or break agentic ai adoption. 
        2. Solution - ToolGuards. Offline step to generate the toolguards from the companies tool repository. If a planned action violates a policy, the agent is prompted to selfreflect and revise its plan before proceeding
12. Problem - In multi-turn conversational environments like τ -bench (Tau Bench), these agents often struggle with consistent reasoning, adherence to domain-specific policies, and extracting correct information over a long horizon of tool-calls and conversation.
    1. Solution - we propose the Input-Reformulation Multi-Agent (IRMA) framework, which automatically reformulates user queries augmented with relevant domain rules and tool suggestions for the tool-calling agent to focus on.
    2. ![Img](img1.png)
    3. https://arxiv.org/pdf/2508.20931
13. Problem - Tool overuse. Models could have solved using parametric knowledge, but still refer to external tools. when should the agent rely on external tools versus its own knowledge? LLMs unnecessarily invoking tools over 30% of the time
    1. Solution - SMART (Strategic Model-Aware Reasoning with Tools). Empirical results show that SMARTAgent reduces tool use by 24% while improving overall performance by over 37%, effectively mitigating tool overuse.
    2. ![Img](20260111125930.png)
