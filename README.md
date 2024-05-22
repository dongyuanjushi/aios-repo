# AIOS: LLM Agent Operating System
AIOS, a Large Language Model (LLM) Agent operating system, embeds large language model into Operating Systems (OS) as the brain of the OS, enabling an operating system "with soul" -- an important step towards AGI. AIOS is designed to optimize resource allocation, facilitate context switch across agents, enable concurrent execution of agents, provide tool service for agents, maintain access control for agents, and provide a rich set of toolkits for LLM Agent developers.

**Git clone**

**Make sure you have Python = 3.11**
Install the required packages using pip
```bash
conda create -n AIOS python=3.11
source activate AIOS
cd AIOS
pip install -r requirements.txt
```

### Usage
If you use open-sourced models from huggingface, you need to setup your [Hugging Face token](https://huggingface.co/settings/tokens) and cache directory
```bash
export HUGGING_FACE_HUB_TOKEN=<YOUR READ TOKEN>
export HF_HOME=<YOUR CACHE DIRECTORY>
```
If you use LLM APIs, you need to setup your API key such as [OpenAI API Key](https://platform.openai.com/api-keys), [Gemini API Key](https://aistudio.google.com/app/apikey)
```bash
export OPENAI_API_KEY=<YOUR OPENAI API KEY>
export GEMINI_API_KEY=<YOUR GEMINI API KEY>
```

You can also create .env file from the .env.example file, and then use dotenv to load the environment variables using .env file into your application's environment at runtime.

```bash
cp .env.example .env
```
#### (1) Demonstration Mode
In the demonstration mode, we provide a toy example: we hardcode three agents and allow you to change the parameters. Then you can see the output of each step in running multiple agents
For open-sourced LLMs, you need to setup the name of the LLM you would like to use the max gpu memory, the evaluation device and the maximum length of generated new tokens.
```bash
# For open-sourced LLMs
python main.py --llm_name <llm_name> --max_gpu_memory <max_gpu_memory> --eval_device <eval_device> --max_new_tokens <max_new_tokens>
## Use google/gemma-1.1-2b-it for example
python main.py --llm_name google/gemma-1.1-2b-it --max_gpu_memory '{"0": "24GB"}' --eval_device "cuda:0" --max_new_tokens 256
```
For close-sourced LLMs, you just need to setup the name of the LLM.
```bash
# For close-sourced LLMs
python main.py --llm_name <llm_name>
## Use gpt-4 for example
python main.py --llm_name gpt-4
```

#### (2) Interactive Mode
In the deployment mode, the outputs of running agents are stored in files. And in this mode, you are provided with multiple commands to run agents and see resource usage of agents (e.g., run \<xxxAgent\>: \<YOUR TASK\>, print agent).
Different from the interactive mode, you need to set all the default loggers as file loggers.
```bash
# For open-sourced LLMs
python simulator.py --llm_name <llm_name> --max_gpu_memory <max_gpu_memory> --eval_device <eval_device> --max_new_tokens <max_new_tokens> --scheduler_log_mode file --agent_log_mode file --llm_kernel_log_mode file
## Use google/gemma-1.1-2b-it for example
python simulator.py --llm_name google/gemma-1.1-2b-it --max_gpu_memory '{"0": "24GB"}' --eval_device "cuda:0" --max_new_tokens 256 --scheduler_log_mode file --agent_log_mode file --llm_kernel_log_mode file
```
```bash
# For close-sourced LLMs
python simulator.py --llm_name <llm_name> --scheduler_log_mode file --agent_log_mode file --llm_kernel_log_mode file
## Use gpt-4 for example
python simulator.py --llm_name gpt-4 --scheduler_log_mode file --agent_log_mode file --llm_kernel_log_mode file
```

Instance of available commands
```bash
run NarrativeAgent: Craft a tale about a valiant warrior on a quest to uncover priceless treasures hidden within a mystical island.
print agent
```

#### (3) Evaluation Mode
In the evaluation mode, we allow you to configure different types of predefined agents (MathAgent, NarrativeAgent, RecAgent) with a configurable number of agents for each type. Additionally, you can evaluate the acceleration performance with or without AIOS by comparing the waiting time and turnaround time.
```bash
python eval.py --llm_name gpt-3.5-turbo --agents MathAgent:1,NarrativeAgent:1,RecAgent:1
```
You can use bash script to start the agent execution like this
```bash
bash scripts/eval/gpt4.sh
````
If you want to obtain metrics for either concurrent execution (with AIOS) or sequential execution (without AIOS), you can specify the mode parameter when running the eval.py file."
```bash
python eval.py --llm_name gpt-4 --agents MathAgent:1,NarrativeAgent:1,RecAgent:1 --mode concurrent-only
python eval.py --llm_name gpt-4 --agents MathAgent:1,NarrativeAgent:1,RecAgent:1 --mode sequential-only
```

