# GitHub 热门 Java AI 项目调研报告

> 调研日期：2026年2月9日
> 调研范围：GitHub 上 Star 数最高的 Java + AI 相关项目

---

## 目录

1. [LangChain4j](#1-langchain4j) - Java 版 LangChain
2. [Spring AI Alibaba](#2-spring-ai-alibaba) - 阿里巴巴 AI Agent 框架
3. [Spring AI](#3-spring-ai) - Spring 官方 AI 框架
4. [Deep Java Library (DJL)](#4-deep-java-library-djl) - 深度学习框架
5. [ChatGPT-Java](#5-chatgpt-java) - ChatGPT Java SDK
6. [MCP Java SDK](#6-mcp-java-sdk) - MCP 官方 Java SDK
7. [Jlama](#7-jlama) - Java LLM 推理引擎
8. [Vespa](#8-vespa) - AI + 数据在线平台
9. [OptaPlanner](#9-optaplanner) - AI 约束求解器
10. [Agents-Flex](#10-agents-flex) - 轻量级 AI 开发框架

---

## 1. LangChain4j

**GitHub**: https://github.com/langchain4j/langchain4j
**Stars**: ⭐ 10,686

### 项目简介
LangChain4j 是 Java 版的 LangChain，简化 LLM 与 Java 应用的集成。

### 核心功能
- **统一 API**：支持 20+ LLM 提供商（OpenAI、Google、Anthropic 等）
- **向量数据库**：支持 30+ 向量存储（Pinecone、Milvus、Chroma 等）
- **RAG 支持**：完整的检索增强生成管道
- **Tool Calling**：函数调用和 MCP 支持
- **Agent 模式**：智能代理开发

### 项目架构
```
┌─────────────────────────────────────────┐
│           Application Layer             │
├─────────────────────────────────────────┤
│  AI Services (Chat, RAG, Agent, etc.)   │
├─────────────────────────────────────────┤
│     LangChain4j Unified API Layer       │
├──────────────────┬──────────────────────┤
│   LLM Providers  │   Vector Stores      │
│  (OpenAI, etc.)  │  (Pinecone, etc.)    │
└──────────────────┴──────────────────────┘
```

### 应用场景
- 智能客服聊天机器人
- 企业知识库问答系统
- 文档智能分析处理
- AI 辅助编程工具

### 代码示例
```java
// 简单对话
ChatLanguageModel model = OpenAiChatModel.builder()
    .apiKey("your-api-key")
    .modelName("gpt-4")
    .build();

String answer = model.generate("Java 有什么优势？");

// RAG 检索增强
EmbeddingStore<TextSegment> store = new InMemoryEmbeddingStore<>();
ContentRetriever retriever = EmbeddingStoreContentRetriever.from(store);

Assistant assistant = AiServices.builder(Assistant.class)
    .chatLanguageModel(model)
    .contentRetriever(retriever)
    .build();
```

---

## 2. Spring AI Alibaba

**GitHub**: https://github.com/alibaba/spring-ai-alibaba
**Stars**: ⭐ 8,334

### 项目简介
阿里巴巴出品的企业级 AI Agent 开发框架，支持多智能体编排。

### 核心功能
- **多 Agent 编排**：SequentialAgent、ParallelAgent、RoutingAgent
- **上下文工程**：Human-in-the-loop、上下文压缩
- **Graph 工作流**：基于图的工作流运行时
- **A2A 通信**：Agent 间分布式协作
- **可视化平台**：一站式 Agent 开发平台

### 项目架构
```
┌─────────────────────────────────────────┐
│        Spring AI Alibaba Admin          │
│    (可视化开发、监控、评估、MCP管理)      │
├─────────────────────────────────────────┤
│      Agent Framework (多智能体框架)      │
│  Sequential | Parallel | Routing | Loop │
├─────────────────────────────────────────┤
│         Graph Runtime (图运行时)         │
│    持久化 | 工作流编排 | 流式处理         │
├─────────────────────────────────────────┤
│            Spring AI Core               │
└─────────────────────────────────────────┘
```

### 应用场景
- 企业级智能客服系统
- 复杂业务流程自动化
- 多模态 AI 应用开发
- 分布式 AI Agent 协作

### 代码示例
```java
// 添加依赖
// spring-ai-alibaba-agent-framework
// spring-ai-alibaba-starter-dashscope

// 创建简单 ChatBot
@RestController
public class ChatController {
    @Autowired
    private ChatClient chatClient;
    
    @GetMapping("/chat")
    public String chat(@RequestParam String message) {
        return chatClient.prompt()
            .user(message)
            .call()
            .content();
    }
}
```

---

## 3. Spring AI

**GitHub**: https://github.com/spring-projects/spring-ai
**Stars**: ⭐ 7,880

### 项目简介
Spring 官方 AI 工程框架，将 AI 能力无缝集成到 Spring 生态。

### 核心功能
- **多模型支持**：Chat、Embedding、Image、Audio、Moderation
- **向量数据库**：支持主流向量存储
- **结构化输出**：AI 输出映射到 POJO
- **Tool Calling**：函数调用能力
- **可观测性**：AI 操作监控

### 项目架构
```
┌─────────────────────────────────────────┐
│         Spring Boot Application         │
├─────────────────────────────────────────┤
│    ChatClient API | Advisors API        │
├─────────────────────────────────────────┤
│  Chat | Embedding | Image | Audio APIs  │
├──────────────────┬──────────────────────┤
│   AI Providers   │   Vector Stores      │
│ OpenAI/Anthropic │  PGVector/Milvus     │
└──────────────────┴──────────────────────┘
```

### 应用场景
- Spring Boot AI 应用快速开发
- 企业级 RAG 系统
- AI 驱动的微服务
- 多模态内容生成

### 代码示例
```java
@RestController
public class AiController {
    private final ChatClient chatClient;
    
    public AiController(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }
    
    @GetMapping("/ai")
    public String generate(@RequestParam String message) {
        return chatClient.prompt()
            .user(message)
            .call()
            .content();
    }
}
```

---

## 4. Deep Java Library (DJL)

**GitHub**: https://github.com/deepjavalibrary/djl
**Stars**: ⭐ 4,767

### 项目简介
亚马逊开源的引擎无关深度学习框架，让 Java 开发者轻松使用 ML/DL。

### 核心功能
- **引擎无关**：支持 PyTorch、TensorFlow、MXNet、ONNX
- **模型动物园**：预训练模型库
- **自动硬件选择**：CPU/GPU 自动切换
- **训练与推理**：完整的 ML 生命周期支持

### 项目架构
```
┌─────────────────────────────────────────┐
│           DJL High-Level API            │
│    (Criteria, Predictor, Trainer)       │
├─────────────────────────────────────────┤
│           Model Zoo (预训练模型)          │
├─────────────────────────────────────────┤
│         Engine Abstraction Layer        │
├─────┬─────┬─────┬─────┬─────────────────┤
│PyTorch│ TF │MXNet│ONNX │  其他引擎       │
└─────┴─────┴─────┴─────┴─────────────────┘
```

### 应用场景
- 图像分类与目标检测
- 自然语言处理
- 推荐系统
- 时间序列预测

### 代码示例
```java
// 目标检测推理
Criteria<Image, DetectedObjects> criteria = Criteria.builder()
    .optApplication(Application.CV.OBJECT_DETECTION)
    .setTypes(Image.class, DetectedObjects.class)
    .optFilter("backbone", "resnet50")
    .build();

Image img = ImageFactory.getInstance().fromUrl("http://...");
try (ZooModel<Image, DetectedObjects> model = criteria.loadModel();
     Predictor<Image, DetectedObjects> predictor = model.newPredictor()) {
    DetectedObjects result = predictor.predict(img);
}
```

---

## 5. ChatGPT-Java

**GitHub**: https://github.com/PlexPt/chatgpt-java
**Stars**: ⭐ 3,631

### 项目简介
开箱即用的 ChatGPT Java SDK，支持 GPT-4o、GPT-5 等最新模型。

### 核心功能
- **多模型支持**：GPT-3.5、GPT-4、GPT-4o、GPT-5
- **流式对话**：SSE 实时响应
- **函数调用**：Function Calling 支持
- **Token 计算**：自动计算消耗
- **代理支持**：HTTP/SOCKS5 代理

### 项目架构
```
┌─────────────────────────────────────────┐
│          Application Layer              │
├─────────────────────────────────────────┤
│   ChatGPT / ChatGPTStream Client        │
├─────────────────────────────────────────┤
│    Message | ChatCompletion Builder     │
├─────────────────────────────────────────┤
│      HTTP Client (OkHttp/Retrofit)      │
├─────────────────────────────────────────┤
│           OpenAI API / 代理              │
└─────────────────────────────────────────┘
```

### 应用场景
- 智能对话机器人
- AI 写作助手
- 代码生成工具
- 智能客服系统

### 代码示例
```java
// 基础对话
ChatGPT chatGPT = ChatGPT.builder()
    .apiKey("sk-xxx")
    .proxy(Proxys.http("127.0.0.1", 1080))
    .build()
    .init();

String res = chatGPT.chat("写一首关于春天的诗");

// 流式对话
ChatGPTStream stream = ChatGPTStream.builder()
    .apiKey("sk-xxx")
    .build()
    .init();

stream.streamChatCompletion(chatCompletion, new ConsoleStreamListener());
```

---

## 6. MCP Java SDK

**GitHub**: https://github.com/modelcontextprotocol/java-sdk
**Stars**: ⭐ 3,154

### 项目简介
Model Context Protocol 官方 Java SDK，与 Spring AI 团队合作维护。

### 核心功能
- **MCP 客户端**：连接 MCP 服务器
- **MCP 服务器**：暴露 MCP 端点
- **同步/异步**：支持两种通信模式
- **Spring 集成**：Spring Boot Starter 支持

### 项目架构
```
┌─────────────────────────────────────────┐
│         Spring AI MCP Starters          │
├─────────────────────────────────────────┤
│    MCP Client API | MCP Server API      │
├─────────────────────────────────────────┤
│      Transport Layer (HTTP/STDIO)       │
├─────────────────────────────────────────┤
│    Jackson JSON | Reactive Streams      │
└─────────────────────────────────────────┘
```

### 应用场景
- AI 工具集成
- 跨应用 AI 能力共享
- 企业 AI 服务标准化
- IDE AI 插件开发

### 代码示例
```java
// MCP 服务器配置
@Configuration
public class McpServerConfig {
    @Bean
    public McpServer mcpServer() {
        return McpServer.builder()
            .tool("search", this::searchTool)
            .tool("calculate", this::calculateTool)
            .build();
    }
}
```

---

## 7. Jlama

**GitHub**: https://github.com/tjake/Jlama
**Stars**: ⭐ 1,249

### 项目简介
纯 Java 实现的现代 LLM 推理引擎，支持本地运行大模型。

### 核心功能
- **多模型支持**：Llama、Mistral、Gemma、Qwen 等
- **量化支持**：Q4、Q8、F16、BF16
- **分布式推理**：集群模式
- **OpenAI 兼容 API**：REST API 服务

### 项目架构
```
┌─────────────────────────────────────────┐
│      REST API / CLI Interface           │
├─────────────────────────────────────────┤
│         Model Loading Layer             │
│    (SafeTensors, Tokenizers)            │
├─────────────────────────────────────────┤
│      Inference Engine (GEMM Ops)        │
├─────────────────────────────────────────┤
│  Panama Vector | Native SIMD | WebGPU   │
└─────────────────────────────────────────┘
```

### 应用场景
- 本地 AI 应用开发
- 离线 LLM 推理
- 边缘设备 AI 部署
- 隐私敏感场景

### 代码示例
```java
// 本地模型推理
File modelPath = new Downloader("./models", "tjake/Llama-3.2-1B-Instruct-JQ4")
    .huggingFaceModel();

AbstractModel model = ModelSupport.loadModel(modelPath, DType.F32, DType.I8);

PromptContext ctx = model.prompt()
    .addSystemMessage("你是一个有帮助的助手")
    .addUserMessage("Java 21 有什么新特性？")
    .build();

Generator.Response r = model.generate(UUID.randomUUID(), ctx, 0.7f, 256, (s, f) -> {});
System.out.println(r.responseText);
```

---

## 8. Vespa

**GitHub**: https://github.com/vespa-engine/vespa
**Stars**: ⭐ 6,766

### 项目简介
雅虎开源的 AI + 数据在线平台，支持大规模实时搜索和推理。

### 核心功能
- **向量搜索**：高性能向量检索
- **实时推理**：毫秒级 ML 模型推理
- **混合搜索**：向量 + 文本 + 结构化数据
- **分布式架构**：自动分片和负载均衡

### 项目架构
```
┌─────────────────────────────────────────┐
│           Query/Feed API                │
├─────────────────────────────────────────┤
│    Container (Query Processing)         │
├─────────────────────────────────────────┤
│   Content (Distributed Storage)         │
├─────────────────────────────────────────┤
│  Proton (Search/Index/ML Inference)     │
└─────────────────────────────────────────┘
```

### 应用场景
- 大规模搜索引擎
- 实时推荐系统
- 个性化内容分发
- RAG 向量检索

### 代码示例
```java
// Vespa 应用配置 (services.xml)
<services>
  <container id="default" version="1.0">
    <search/>
    <document-api/>
  </container>
  <content id="content" version="1.0">
    <documents>
      <document type="article" mode="index"/>
    </documents>
  </content>
</services>
```

---

## 9. OptaPlanner

**GitHub**: https://github.com/apache/incubator-kie-optaplanner
**Stars**: ⭐ 3,477

### 项目简介
Apache 孵化的 AI 约束求解器，解决复杂规划调度问题。

### 核心功能
- **约束求解**：基于约束的优化
- **多种算法**：遗传算法、模拟退火、禁忌搜索
- **增量计算**：高效的增量评分
- **Quarkus 集成**：云原生支持

### 项目架构
```
┌─────────────────────────────────────────┐
│         Problem Definition              │
│   (Planning Entity, Planning Variable)  │
├─────────────────────────────────────────┤
│      Constraint Stream API              │
├─────────────────────────────────────────┤
│         Solver Engine                   │
│  (Construction | Local Search | Meta)   │
├─────────────────────────────────────────┤
│      Score Calculation                  │
└─────────────────────────────────────────┘
```

### 应用场景
- 车辆路径规划 (VRP)
- 员工排班调度
- 会议室/资源分配
- 生产计划优化

### 代码示例
```java
// 定义规划实体
@PlanningEntity
public class Task {
    @PlanningVariable
    private Employee employee;
    
    @PlanningVariable
    private LocalDateTime startTime;
}

// 定义约束
Constraint noOverlap(ConstraintFactory factory) {
    return factory.forEach(Task.class)
        .join(Task.class, equal(Task::getEmployee))
        .filter((t1, t2) -> t1.overlaps(t2))
        .penalize(HardSoftScore.ONE_HARD)
        .asConstraint("任务不能重叠");
}
```

---

## 10. Agents-Flex

**GitHub**: https://github.com/agents-flex/agents-flex
**Stars**: ⭐ 1,337

### 项目简介
轻量级 Java AI 应用开发框架，简单易用。

### 核心功能
- **LLM 访问**：多模型支持
- **Tool 调用**：工具定义与执行
- **MCP 支持**：MCP 协议集成
- **向量存储**：Embedding 和检索
- **可观测性**：OpenTelemetry 集成

### 项目架构
```
┌─────────────────────────────────────────┐
│         Application Layer               │
├─────────────────────────────────────────┤
│   Chat | Tool | Memory | Embedding      │
├─────────────────────────────────────────┤
│      LLM Providers (多模型支持)          │
├─────────────────────────────────────────┤
│    OpenTelemetry (可观测性)              │
└─────────────────────────────────────────┘
```

### 应用场景
- 快速 AI 原型开发
- 轻量级聊天应用
- 工具增强型 Agent
- 企业 AI 集成

### 代码示例
```java
public static void main(String[] args) {
    OpenAIChatModel chatModel = OpenAIChatConfig.builder()
        .provider("GiteeAI")
        .endpoint("https://ai.gitee.com")
        .apiKey("your-api-key")
        .model("Qwen3-32B")
        .buildModel();

    String output = chatModel.chat("如何提高代码质量？");
    System.out.println(output);
}
```

---

## 总结对比

| 项目 | Stars | 定位 | 适用场景 |
|-----|-------|------|---------|
| LangChain4j | 10.6k | LLM 集成框架 | 通用 AI 应用 |
| Spring AI Alibaba | 8.3k | 企业级 Agent | 复杂业务流程 |
| Spring AI | 7.9k | Spring AI 框架 | Spring 生态 |
| Vespa | 6.8k | AI + 数据平台 | 大规模搜索 |
| DJL | 4.8k | 深度学习框架 | ML/DL 应用 |
| ChatGPT-Java | 3.6k | ChatGPT SDK | 快速集成 |
| OptaPlanner | 3.5k | 约束求解器 | 规划调度 |
| MCP Java SDK | 3.2k | MCP 协议 | 工具集成 |
| Agents-Flex | 1.3k | 轻量级框架 | 快速原型 |
| Jlama | 1.2k | 本地推理引擎 | 离线部署 |

---

## 推荐选择

- **快速开发 AI 应用** → LangChain4j / Spring AI
- **企业级多 Agent 系统** → Spring AI Alibaba
- **本地/离线 LLM** → Jlama
- **深度学习训练推理** → DJL
- **大规模搜索推荐** → Vespa
- **规划调度优化** → OptaPlanner

---

*报告完成于 2026年2月9日*
