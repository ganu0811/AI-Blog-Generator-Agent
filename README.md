# AI-Blog-Generator-Agent

An intelligent blog generation system built with LangChain and LangGraph that automatically creates, reviews, and refines blog posts using AI agents.

## Overview

This project implements an agentic AI workflow that generates high-quality blog posts through a multi-step process:

1. **Title Creator**: Generates catchy titles based on given topics
2. **Content Creator**: Writes detailed blog content with introduction, main body, and conclusion
3. **Editor**: Reviews and provides feedback on the generated content
4. **Iterative Refinement**: Automatically improves content based on editor feedback

## Features

- **Multi-Agent Architecture**: Uses LangGraph to orchestrate different AI agents
- **Quality Control**: Built-in editor agent ensures content meets quality standards
- **Iterative Improvement**: Automatically refines titles and content until approved
- **Structured Output**: Generates blogs with proper introduction, body, and conclusion sections
- **Configurable Models**: Uses Groq Model for content generation

## Prerequisites

- Python 3.8+
- OpenAI API key
- Groq API key (optional, based on your setup)

## Installation

1. Clone this repository:
```bash
git clone https://github.com/yourusername/AI-Blog-Generator-Agent.git
cd AI-Blog-Generator-Agent
```

2. Install required dependencies:
```bash
pip install langchain-openai langgraph python-dotenv
```

3. Create a `.env` file in the root directory and add your API keys:
```env
OPENAI_API_KEY=your_openai_api_key_here
groq_api_key=your_groq_api_key_here
```

## Usage

1. Open the Jupyter notebook `AI_blog_generator.ipynb`

2. Run all cells to set up the environment and create the blog generation graph

3. Generate a blog post by providing a topic:
```python
messages = {"messages": HumanMessage(content="Write a blog post about playing outdoor sports")}
thread = {'configurable': {'thread_id': '1'}}

for event in blog_graph.stream(messages, thread, stream_mode='values'):
    event['messages'][-1].pretty_print()
```

## Architecture

### State Management
The system uses a `MessagesState` that maintains conversation history throughout the generation process.

### Agents

- **Title Creator**: Generates engaging titles based on the input topic
- **Content Creator**: Creates structured blog content with:
  - Introduction (50-75 words)
  - Main body (350 words with technical details/solutions)
  - Conclusion (50-75 words)
- **Editor**: Reviews content and provides feedback:
  - `approved`: Content is good to go
  - `change_title`: Title needs improvement
  - `change_content`: Main content needs refinement

### Workflow

The system follows a conditional workflow where content is iteratively improved until the editor approves it:

```
START → Title Creator → Content Creator → Editor → Decision
                ↑                              ↓
                └─────── Change Title ←────────┘
                ↑                              ↓
                └─────── Improve Content ←─────┘
                                              ↓
                                            END
```

## Configuration

- **Interrupt Points**: The system pauses before the editor for human review if needed
- **Memory**: Uses MemorySaver for conversation persistence
- **Thread Management**: Supports multiple concurrent blog generation sessions

## Example Output

The system generates structured blog posts with:
- Compelling titles tailored to the topic
- Well-organized content with clear sections
- Technical depth appropriate for the subject matter
- Professional conclusion that reinforces key points

## Customization

You can customize the system by:
- Modifying the system messages in each agent for different writing styles
- Adjusting word count requirements for different sections
- Adding new agents for specific content requirements (e.g., SEO optimization)
- Changing the LLM model for different performance characteristics

## Dependencies

- `langchain-openai`: For OpenAI model integration
- `langgraph`: For agent orchestration and workflow management
- `python-dotenv`: For environment variable management
- `typing-extensions`: For enhanced type annotations
