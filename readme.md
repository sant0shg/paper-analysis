# Problem statement 1 - Analysis of short form videos

Design and evaluate an end-to-end, multi-modal pipeline that automatically extracts **fashion and pricing trends** from short-form product videos (e.g., YouTube/Instagram), inspired by the methods in:

https://link.springer.com/article/10.1007/s00521-025-11218-1

The system should jointly analyze audio (speech), visual frames, and on-screen text to detect emerging trends (e.g., "linen shirts under ₹200"), with a focus on **robustness, scalability, and research-grade evaluation** suitable for an M.Tech project.

## Tentative solution and packages

The pipeline is decomposed into stages, each mapped to concrete libraries/models:

| **Stage / Task**            | **Role in pipeline**                                             | **Recommended Library/Model**                |
| --------------------------- | ---------------------------------------------------------------- | -------------------------------------------- |
| **Pipeline Orchestration**  | Download videos, manage steps, export clips/keyframes           | MoviePy or FFmpeg                            |
| **Transcription**           | Generate time-aligned transcript from audio                      | Whisper (local or API)                       |
| **Visual Keyframe Capture** | Extract keyframes around scene changes / product highlights      | OpenCV (e.g., `cv2.absdiff`, SSIM, hist diff) |
| **Visual Understanding**    | Interpret outfits, objects, brand cues from keyframes            | LLaVA or GPT-4V                              |
| **OCR / Logo Detection**    | Read on-screen text, detect brand logos                          | EasyOCR, YOLOv8                              |
| **Trend Graph Construction**| Aggregate entities and prices across many videos into a trend map| NetworkX                                     |
| **Sentiment / Intent**      | Capture sentiment or persuasive tone in speech/text              | TextBlob or VADER                            |

High-level flow:

1. Ingest a batch of videos and extract audio + candidate keyframes.
2. Run Whisper to obtain a timestamped transcript.
3. For each segment, align keyframes with transcript spans (temporal fusion).
4. Apply vision (LLaVA / GPT-4V) + OCR + logo detection to extract products, brands, and prices.
5. Build a graph of products, attributes, and price ranges across videos (NetworkX).
6. Compute simple metrics (frequency, co-occurrence, growth) to identify candidate "trends".

## Future scope

To elevate this from a basic pipeline to a strong M.Tech-level research project:

- **Advanced keyframe extraction**: Replace fixed-interval frames with shot-boundary / semantic-shift detection (histogram difference, SSIM, or transformer-based shot detection) to reduce redundant visual queries.
- **Temporal multi-modal fusion**: Design an explicit alignment layer that reasons jointly over transcript spans + nearby frames, and study how this affects trend detection accuracy.
- **Agentic verification loop**: Add a "search agent" that cross-checks discovered trends against external sources (e.g., YouTube/Google search APIs) to distinguish real trends from noise.
- **Performance metrics & benchmarks**: Create a labeled dataset (e.g., 50–100 videos with manually annotated trends) and report precision/recall/F1 for trend detection and product classification across different model choices (Whisper sizes, LLaVA vs GPT-4V, etc.).
- **Scalability experiments**: Measure runtime and cost as the number of videos and frame queries grow; explore batching, caching, and model distillation strategies.

# Problem statement 2 - Agentic AI planning

1. Problem - Evaluation of reasoning/planning capabilities of LLM 
	1. Solution - PlanBench
	2. https://arxiv.org/pdf/2206.10498
2. Problem - LLMs show promise in leveraging external tools. But planning consumes large amount of tokens. 
	1. Solution - During post training, DEEPPLANNER uses token-level advantage with an entropy-based term to allocate larger updates to high entropy tokens, and selectively upweights sample-level advantages for planning-intensive rollouts. 
	2. They dont just do supervised fine tuning, SFT, but use RL based fine tuning. 
	3. https://arxiv.org/pdf/2510.12979
	4. https://github.com/AlexFanw/DeepPlanner/blob/main/user/train.sh
3. Problem - Long term context is missing. LLMs should be able to hold conversational systems for long term interactions and tasks. Proactiveness in LLMs are missing. They do not ask questions about you when you ask "Plan a travel".
	1. Solution - Uses meta prompt, the macro-actions are instantiated to "ask a question", "add a step", or "alter a step", so that the AI agent can operate in a similar fashion as a person would if they were to be asked to help with coaching or tutoring or assisting with making a plan. 
	2. https://arxiv.org/pdf/2502.19500
4. Problem - Due to bad reasoning, the LLMs face issue during complex planning problems. 
	1. Solution - PlanGen. Three agents - planning agent, constraint agent and verification agent. 
	2. https://arxiv.org/pdf/2502.16111
5. Problem - Long term plan is not possible for LLM. 
	1. Solution - Feedback aware fine tuning (FAFT) vs Supervised Fine Tuning (SFT), why FAFT is better
	2. https://arxiv.org/pdf/2408.06318
6. Problem - Gaming related parameterized skills, can LLM work
	1. Solution - Skill library, skill planner and skill executor implement PLAP in MicroRTS
	2. https://arxiv.org/pdf/2509.13127
7. Problem - Increasing context of LLM
	1. Solution - Instead of RAG, we use Recursive Language Model. LLM spaws sub child to find the right data. It can be script or it can be something else. 
	2. https://arxiv.org/pdf/2512.24601
8. Problem - COT and REACT has shown good capabilities in LLM for reasoning, but still there is issues. Issues like constraint violations, inconsistent state tracking, and brittle solutions that break under minor changes
	1. Solution - Model First Reasoning. Error arises from inconsistent models. Hallucination is not merely the generation of false statements. Rather, it is a symptom of reasoning performed without a clearly defined model of the problem space.
	2. ![Img](20260111204715.png)
	3. https://arxiv.org/pdf/2512.14474
9. Problem - LLMs suffer from halliculations, outdated knowledge, high training and inference cost.  
	1. Solution - Survery of all methods
	2. https://arxiv.org/pdf/2508.17692
10. Problem - 
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
