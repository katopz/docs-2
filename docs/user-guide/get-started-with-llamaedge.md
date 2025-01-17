---
sidebar_position: 2
---

# LlamaEdge step-by-step

Let's dive into a simple and practical tutorial on getting started with LlamaEdge, focusing on how to use a Command Line Interface (CLI) installer to run a model, along with some useful WasmEdge commands. This guide can be adjusted and applied to run Llama 2 series of models, tailored to give you a hands-on approach to running your large language model with LlamaEdge.

### Step 1: Install WasmEdge

First off, you'll need WasmEdge, a high-performance, lightweight, and extensible WebAssembly (Wasm) runtime optimized for server-side and edge computing. To install WasmEdge along with the necessary plugin for AI inference, open your terminal and execute the following command:

```
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install_v2.sh | bash -s
```

This command fetches and runs the WasmEdge installation script, which automatically installs WasmEdge and the WASI-NN plugin, essential for running LLM models like Llama 3.

### Step 2: Download the LLM model

Next, you'll need to obtain a model file. For this tutorial, we're focusing on the **Llama 3 8B model finetuned for instruction following**, but the steps are generally applicable to other models too. Use the following command to download the model file.

```
curl -LO https://huggingface.co/second-state/Llama-3-8B-Instruct-GGUF/resolve/main/Meta-Llama-3-8B-Instruct-Q5_K_M.gguf
```

This command downloads the Llama-3-8B-Instruct model from Huggingface, an AI model hosting platform.

### Step 3: Download a portable chatbot app

Next, you need an application that can load the model and provide an UI to interact with the model.
The [LlamaEdge CLI chat app](https://github.com/LlamaEdge/LlamaEdge/tree/main/chat) is a lightweight and cross-platform Wasm app that works on any device
you might have. Just download the compiled binary app.

```
curl -LO https://github.com/second-state/LlamaEdge/releases/latest/download/llama-chat.wasm
```

> The LlamaEdge apps are written in Rust and compiled to portable Wasm. That means they can run across devices and OSes without any change to the binary apps. You can simply download and run the compiled wasm apps regardless of your platform.

### Step 4: Chat with the Model

With everything set up, it's time to run the chat app with the LLM model as follows.

```
wasmedge --dir .:. --nn-preload default:GGML:AUTO:Meta-Llama-3-8B-Instruct-Q5_K_M.gguf llama-chat.wasm -p llama-3-chat
```

This command executes the chat application, allowing you to start interacting with the Llama 2 13b model. Here, `wasmedge` is the command to run the WasmEdge runtime, `--nn-preload` specifies the model to use with the WASI-NN plugin, and `-p` sets the prompt template for the chat.

### Step 5: Chat with the chatbot UI 

The command line UI is nice, but most people would perfer a web UI. The web UI also allows you to make your
local LLM accessible to other people across the network.
To do that, you need the [LlamaEdge API server](https://github.com/LlamaEdge/LlamaEdge/tree/main/api-server) app.

```
curl -LO https://github.com/second-state/LlamaEdge/releases/latest/download/llama-api-server.wasm
```

The `llama-api-server.wasm` is a web server with an OpenAI compaitble API. You still need HTML files for the chatbot UI.
Download and unzip the HTML UI files as follows.

```
curl -LO https://github.com/LlamaEdge/chatbot-ui/releases/latest/download/chatbot-ui.tar.gz
tar xzf chatbot-ui.tar.gz
rm chatbot-ui.tar.gz
```

Then, start the web server.

```
wasmedge --dir .:. --nn-preload default:GGML:AUTO:Meta-Llama-3-8B-Instruct-Q5_K_M.gguf llama-api-server.wasm -p llama-3-chat
```

Go to `http://localhost:8080` on your computer to access the chatbot UI on a web page!

