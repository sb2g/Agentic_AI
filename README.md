# Overview:
- This repo goes over what are Agents & their components/workflows/concepts/frameworks along with examples.
    - `AgenticAI_ConceptsExamples_1`: 
        - Agentic AI concepts & examples from [1]. 
        - Includes from scratch agentic workflows, How to create/use tools, Evaluate agentic workflows, Build autonomous single & multi agentic workflows, etc.
        - Mostly based on Python workflows and agnostic to Agentic frameworks. Uses AISuite and OpenAI SDK, along with different tools.
    - `AgenticAI_ConceptsExamples_2`: 
        - Agentic AI example from scratch in Python from [4].
        - Agentic AI concepts & examples from [2,3] along with what are different frameworks, etc.  
    - Different Agent frameworks like:   
        - `LangGraph`  
        - `SmolAgents`  
        - `CrewAI`   

# Trade-off: Control (vs) Freedom
When designing AI applications, there is a trade-off between control and freedom:
- Control allows you to ensure predictable behavior and maintain guardrails.
- Freedom gives AI model to be more room to be creative and tackle unexpected problems.

# Some frameworks to build Agentic workflows
- `LangGraph`
    - Focuses on "Controlling" the execution of agent
    - LangGraph provides the structure you need
    - Useful if your application involves a series of steps that need to be orchestrated in a specific way, with decisions being made at each junction point
- `smolagents`
    - Focuses on "Freedom" of execution of agent
    - Focuses on Code Agent
    - Can call multiple tools in a single action step, create their own tools, etc. However, this behavior can make them less predictable and less controllable than a regular Agent working with JSON
- `CrewAI`
    - Useful to build multi-agent workflows.

### References:
1. https://learn.deeplearning.ai/courses/agentic-ai
2. https://huggingface.co/learn/agents-course/unit1/what-are-agents
3. https://huggingface.co/docs/smolagents/conceptual_guides/intro_agents
4. https://www.deeplearning.ai/short-courses/ai-agents-in-langgraph/  
