# LLM Fundamentals

A collection of foundational Python scripts demonstrating core Large Language Model (LLM) concepts, structured outputs, and tool integration.

## Project Structure

- `1_llm_basics.py`: Introduction to basic API calls and interacting with language models.
- `2_structured_outputs.py`: Guide and implementation for enforcing structured schemas (JSON outputs) from models.
- `3_tool_calls.py`: Examples of defining and handling tool/function calling workflows.

  > **Disclaimer:** The weather tool in `3_tool_calls.py` returns mock/simulated data for demonstration purposes and does not make live calls to a real-world meteorological API.

## Prerequisites

- Python 3.8+
- An API key for your chosen LLM provider (e.g., OpenAI, Google GenAI, etc.)

## Getting Started

1. Clone the repository:
   ```bash
   git clone [https://github.com/your-username/llm_fundamentals.git](https://github.com/your-username/llm_fundamentals.git)
   cd llm_fundamentals
2. Set up a virtual environment and activate it:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate

## Structured Outputs vs. Tool Calling

While both **Structured Outputs** and **Tool Calling** (Function Calling) allow you to interact with Large Language Models in a deterministic, programmatic way, they serve distinct architectural purposes and operate differently under the hood.

### 1. Structured Outputs
* **Primary Goal:** Enforce that the model's response adheres strictly to a predefined schema (e.g., a Pydantic model or JSON Schema).
* **How it Works:** Constrains the decoding/sampling process at the token level (using techniques like grammar-guided generation or guided decoding) so the model physically cannot output invalid JSON or deviate from the specified fields.
* **Use Cases:** Data extraction, classification, sentiment analysis, generating form data, or parsing unformatted text into a strict data model.

### 2. Tool Calling (Function Calling)
* **Primary Goal:** Enable the model to choose and invoke external capabilities, APIs, or databases to fetch real-time information or perform actions.
* **How it Works:** You provide the model with a set of tool definitions (name, description, parameters). The model analyzes the user prompt, decides *if* a tool is needed, and outputs a structured payload containing the tool name and arguments. Your application then executes the function and feeds the result back to the model.
* **Use Cases:** Retrieving weather data, querying a SQL database, calculating math via code execution agents, or interacting with external services.

---

### Architecture & Execution Flow

```text
+-------------------------------------------------------------------------+
|                              USER PROMPT                                |
+-------------------------------------------------------------------------+
                                     |
                  +------------------+------------------+
                  |                                     |
                  v                                     v
     [ Structured Outputs Flow ]             [ Tool Calling Flow ]
                  |                                     |
                  v                                     v
      Model constrained by schema            Model decides to invoke tool
    (Guaranteed schema compliance)          (Returns tool call + arguments)
                  |                                     |
                  v                                     v
      [ Valid JSON Output Returned ]         [ Application Executes Tool ]
                                                        |
                                                        v
                                             [ Tool Output Sent Back ]
                                                        |
                                                        v
                                             [ Final Answer to User ]
