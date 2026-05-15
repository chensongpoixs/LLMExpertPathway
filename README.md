# LLMExpertPathway

# 算核工坊：LLMOps 四阶实战

## 阶段一：基础设施与底层算力（基础筑基期）
**目标**：能够独立搭建一套支持 GPU 运行的容器化环境，理解算力瓶颈在哪里。

### 1. Linux 与 GPU 驱动
- **核心知识**：
  - NVIDIA Driver → CUDA Toolkit → cuDNN 的安装与版本匹配。
  - `nvidia-smi` 指标分析：显存占用、计算利用率、功率、温度。
- **实操**：在 Linux 服务器上完整配置一套 CUDA 环境，确保 PyTorch 能正确识别 GPU。

### 2. 容器化与编排（K8s + Docker）
- **核心知识**：
  - Docker：编写高效的 AI 镜像（减少层数、优化基础镜像）。
  - NVIDIA Container Toolkit：让容器能够调用宿主机 GPU。
  - K8s GPU 调度：学习 `resources: limits: nvidia.com/gpu: 1` 的调度逻辑。
- **实操**：使用 Docker 部署一个简单的模型 Demo → 尝试用 K8s 部署一个可自动扩缩容的推理服务。

## 阶段二：高效推理与服务化（核心突破期）
**目标**：摆脱简单的 `model.generate()`，能够部署生产级、高吞吐的推理接口。

### 1. 现代化推理框架（重点：vLLM）
- **核心知识**：
  - **PagedAttention**：深入理解如何解决 KV Cache 碎片化问题（vLLM 的核心）。
  - **Continuous Batching**：理解为何它比传统 Batching 吞吐量高得多。
  - **KV Cache**：理解其对显存的占用逻辑及清理机制。
- **实操**：使用 vLLM 部署一个 Llama-3 或 Qwen-2 模型，对比它与原版 HuggingFace Transformers 的推理速度。

### 2. 推理加速技术
- **核心知识**：
  - **量化（Quantization）**：学习 FP16 → INT8 → INT4 的演进。掌握 GPTQ, AWQ, GGUF 方案。
  - **投机采样（Speculative Decoding）**：理解用小模型预判、大模型确认的加速原理。
- **实操**：将一个 FP16 的模型量化为 INT4，对比量化前后：显存占用 $\downarrow$ vs 推理速度 $\uparrow$ vs 模型精度 $\downarrow$。

## 阶段三：数据工程与 RAG 架构（能力扩展期）
**目标**：让模型不再“胡说八道”，能够处理私有知识库并实现精准检索。

### 1. 向量化与检索
- **核心知识**：
  - **Embedding 模型**：了解如何将文本转化为高维向量。
  - **向量数据库**：学习 Milvus 或 FAISS 的索引机制（HNSW 索引等）。
- **实操**：搭建一个简单的向量数据库，实现“输入一句话 → 检索出最相关的 3 段文档”的流程。

### 2. 高级 RAG 链路优化
- **核心知识**：
  - **Chunking 策略**：固定长度切分 vs 语义切分。
  - **Rerank（重排）**：理解为什么第一轮检索不够，需要用更强的模型做第二次精选。
  - **Query Transformation**：将用户模糊的提问改写为适合检索的关键词。
- **实操**：构建一个端到端的 RAG 系统：PDF → 切片 → 向量存储 → 检索 → 重排 → LLM 生成答案。

## 阶段四：微调、监控与规模化运维（专家进阶期）
**目标**：实现模型的定制化训练，并建立一套工业级的监控指标体系。

### 1. 参数高效微调（PEFT）
- **核心知识**：
  - **LoRA / QLoRA**：深刻理解“低秩适配”是如何在不改变原权重的情况下实现微调的。
  - **SFT（指令微调）**：如何构建高质量的 $\{Instruction, Input, Output\}$ 数据集。
- **实操**：使用 QLoRA 在单张消费级 GPU 上对模型进行特定任务（如：将模型训练成某个公司的客服语气）的微调。

### 2. LLMOps 监控与治理
- **核心知识**：
  - **性能指标**：监控 TTFT（首字延迟）和 TPS（每秒 Token 数）。
  - **成本核算**：计算单个请求的算力成本 → 制定资源扩容计划。
  - **稳定性**：搭建 Prometheus + Grafana 监控 GPU 显存和温度。
- **实操**：设计一套监控面板，实时显示当前推理服务的负载、延迟分布和显存水位。

## 🛠️ 推荐学习工具清单（必学）

| 类别       | 推荐工具/库                       | 学习优先级     |
|------------|-----------------------------------|----------------|
| 基础环境   | Docker, Kubernetes, NVIDIA-SMI    | P0（必须）     |
| 推理框架   | vLLM, TensorRT-LLM, Ollama        | P0（必须）     |
| 模型库     | Hugging Face Transformers, PEFT   | P0（必须）     |
| 向量数据库 | Milvus, FAISS, ChromaDB           | P1（重要）     |
| 量化工具   | AutoGPTQ, llama.cpp               | P1（重要）     |
| 微调框架   | DeepSpeed, Unsloth, LLaMA-Factory | P2（进阶）     |