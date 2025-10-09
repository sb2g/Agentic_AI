# Overview
- This directory covers key concepts of Agentic AI and has my notebooks from https://learn.deeplearning.ai/courses/agentic-ai

# What is Agentic AI workflow
- Instead of calling an AI model (like LLM) once to excute a task, Agentic workflow executes multiple steps using LLM/VLM and any external tools if necessary.
- Degrees of autonomy
    - Less autonomous: Provide the steps and how the tools should be used explicitly.
    - More autonomous: Provide high level goal and the tools available and let the Agent execute the task.

# Popular Design patterns
- Reflection 
- Tool use
- Planning workflows
- Multi-agent workflows 

# Reflection 
- Check/validate the output LLM 
    - Reflection can be done using same LLM/VLM or different LLM/VLM
    - Use a reflection prompt (e.g. validate code, check for coherence/completeness, etc.)  
- Run the original LLM again using this reflection result, to improve its output
    - Extra information helps to improve the final output (e.g a bug is found, some topic is missing, etc.)

# Tool use
- Some example tools are: Functions (get time), APIs like Web search / Database query, Code engine, etc.
- Basic way to use tools:
    - Prompt the LLM to output a function/API call. 
    - Parse LLM output for the call.
    - Execute it. 
    - Pass the result to LLM prompt 
    - Get the final output from LLM.
- Modern way to use tools:
    - Most LLMs are trained to use tools provided with JSON schema (fields: tool type, name, description, parameters)
    - Libraries like `aisuite` use OpenAI type API calls to use tools.
- Code execution (Special type of Tool use)
    - Prompt LLM to output code between <execute_python> & </execute_python>
    - Parse and execute the code
    - Pass the result to LLM prompt 
    - Get the final output from LLM.
- MCP (Model context protocol)
    - A protocol that lets Client apps (e.g. Cursor, Claude Desktop) call Server tools (Github, Google drive, PostgreSQL) in Agentic AI workflow. 
        - This will allow to reduce number of overall tools out there from `Num_ClientApps * Num_ServerApps` to `Num_ClientApps + Num_ServerApps` 
    - Specialized for AI Agentic tool use, as opposed to generic protocols like REST API. 
    - Usually, the server tools build MCP as a wrapper around REST API.

# Planning workflows
- Planning with tools
    - Prompt LLM to prepare a plan (sequence of steps) to execute a task with given tools  
    - LLM can be prompted to output the plan in JSON format (fields: step, description, tool, args)
- Planning with code execution
    - Prompt the LLM to write a code to execute the task.
    - Similar to the "Code executionn" tool use above, but here its complex sequence of tasks written in code.

# Multi-agent workflows 
- When?
    - Tasks that need more than 1 person
    - Each person performs different task & needs different tools
    - E.g. Marketing team
        - Researcher
            - Tasks: Analyze/Research trends/competitors
            - Tools: Web search
        - Graphic designer
            - Tasks: Create visualizations
            - Tools: Image/Chart generation, Code execution
        - Writer
            - Tasks: Use outputs & create marketing text/copy
            - Tools: None
- Communication patterns
    - Linear plan
        - Outputs directly from one agent to other agent
    - Hierarchical (Deep hierarchical) plan
        - Outputs are passed through manager agent to other agents
    - All-to-all 
        - Each agent talks to any another directly
        - More autonomous/complex, but less predictable/control.

# Evaluation methods & Fixing issues
- Objective vs Subjective evals
    - Objective evals using code
        - Keyword based: 
            - Look for low quality outputs & undesirable keywords in output
        - Use code execution engine
    - Subjective evals
        - Using LLM-as-judge
            - Naive way: Give a score e.g. 1-5
            - Better way: How many times Gold-standard keywords occured, Meaning same, etc.
- Per-example GT vs Single GT evals
    - Each example has GT (e.g. correct date) or there is single GT for all examples (e.g. length of report)
- Component-level vs End-to-end evals 
    - Component-level eval
    - End-to-end eval

# Some tips on build / improving Agentic AI:
- Evals:
    - OK to start with small quick/dirty evals
    - Enable Trace & Scan for failing examples
    - Prepare a table with each example & Count different type of errors
- How to improve?
    - Non-LLM:
        - Tune hyperparams (Web search: Num results, dates. RAG: Similarity threshold, Chunk size. ML models: Thresholds)
        - Try a different provider, algo, model, etc.
    - LLM
        - Improve prompts
        - Try a different model
        - Break it down into smaller steps
        - Fine-tune model
- Latency/Cost
    - LLMs: Per token
    - APIs: Per all
    - Compute: Server capacity/cost
- Iterate between
    - Build
        - E2E System
        - Improve Components
    - Analyze
        - Examine outputs/traces
        - Build evals/metrics
        - Error analysis
        - Component-level evals


# References
1. https://learn.deeplearning.ai/courses/agentic-ai