# Symphonic SDK Usage Guide

## Table of Contents

- [Tools](#tools)
- [Agents](#agents)
- [Teams](#teams)
- [Pipelines](#pipelines)
- [Memory System](#memory-system)
- [Error Handling](#error-handling)
- [Metrics](#metrics)

## Tools

Tools are pure functions with defined inputs and outputs.

```typescript
const myTool = symphony.tools.create({
    name: "myTool",
    description: "Tool description",
    inputs: ["param1", "param2"],
    handler: async (params) => {
        return {
            success: true,
            result: "output"
        };
    }
});
```

### Tool Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Tool identifier |
| `description` | string | Yes | Tool purpose |
| `inputs` | string[] | Yes | Input parameters |
| `handler` | function | Yes | Async implementation |

### Handler Function

The handler can receive parameters in two ways:

```typescript
// Single parameter
handler: async (value) => {
    return { success: true, result: value };
}

// Multiple parameters
handler: async (params) => {
    const { param1, param2 } = params;
    return { success: true, result: output };
}
```

## Agents

Agents use LLMs to orchestrate tools and solve tasks.

```typescript
const myAgent = symphony.agent.create({
    name: "myAgent",
    description: "Agent purpose",
    task: "Specific task",
    tools: [myTool],
    llm: "gpt-4"
});

// Run the agent
const result = await myAgent.run("task description");
```

### Agent Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Agent identifier |
| `description` | string | Yes | Agent purpose |
| `task` | string | Yes | Primary task |
| `tools` | Tool[] | Yes | Available tools |
| `llm` | string | Yes | LLM model |

## Teams

Teams enable agent collaboration and task distribution.

```typescript
const myTeam = symphony.team.create({
    name: "myTeam",
    description: "Team purpose",
    agents: [agent1, agent2],
    manager: true
});

// Run the team
const result = await myTeam.run("complex task");
```

### Team Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Team identifier |
| `description` | string | Yes | Team purpose |
| `agents` | Agent[] | Yes | Team members |
| `manager` | boolean | No | Enable manager |

## Pipelines

Pipelines define sequential workflows with type safety.

```typescript
const myPipeline = symphony.pipeline.create({
    name: "myPipeline",
    description: "Pipeline purpose",
    steps: [
        {
            name: "step1",
            tool: myTool,
            description: "Step purpose",
            chained: 1
        }
    ]
});

// Run the pipeline
const result = await myPipeline.run(input);
```

### Pipeline Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Pipeline identifier |
| `description` | string | Yes | Pipeline purpose |
| `steps` | Step[] | Yes | Workflow steps |

### Step Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Step identifier |
| `tool` | Tool | Yes | Tool to execute |
| `description` | string | Yes | Step purpose |
| `chained` | number | No | Chain position |

## Memory System

Symphonic provides three types of memory:

### Short-Term Memory
```typescript
const memory = symphony.memory.create({
    type: "short_term",
    capacity: 100,
    ttl: 3600 // 1 hour
});

await memory.add("key", "value");
const value = await memory.get("key");
```

### Long-Term Memory
```typescript
const memory = symphony.memory.create({
    type: "long_term",
    capacity: 1000
});

await memory.add("key", "value");
const results = await memory.search("query");
```

### Episodic Memory
```typescript
const memory = symphony.memory.create({
    type: "episodic",
    capacity: 50
});

memory.startEpisode("episode1");
await memory.add("key", "value");
const history = memory.getEpisodeHistory();
memory.endEpisode();
```

## Error Handling

All components use consistent error handling:

```typescript
try {
    const result = await myTool.run(input);
    if (!result.success) {
        console.error(result.error);
    }
} catch (error) {
    console.error(error);
}
```

## Metrics

Built-in metrics tracking:

```typescript
const metrics = symphony.metrics.create();

metrics.trackOperation("operation_name");
metrics.trackModel("model_name");
metrics.trackModelLoad(tokens);

const stats = metrics.getStats();
```

### Available Metrics

- Operation timing
- Model usage
- Token consumption
- Memory usage
- Success rates
- Error counts 
