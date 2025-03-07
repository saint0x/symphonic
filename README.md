# Symphonic SDK

Symphonic is the easiest way to build self-learning tools, agents, teams, and pipelines in minutes.


## Installation

```bash
npm install symphonic
# or
yarn add symphonic
# or
bun add symphonic
```

## Quick Start

```typescript
import { symphony } from "symphonic";

// Create a tool
const searchTool = symphony.tools.create({
    name: "search",
    description: "Search for information",
    inputs: ["query"],
    handler: async (query) => {
        // Implementation
        return { result: searchResults, success: true };
    }
});

// Create an agent
const researcher = symphony.agent.create({
    name: "researcher",
    description: "Research assistant",
    task: "Find and analyze information",
    tools: [searchTool],
    llm: "gpt-4"
});

// Use the agent
const result = await researcher.run("Research quantum computing advances in 2024");
```

## Core Components

### Tools
- Pure functions with defined i/o
- Error handling and metrics
- Async by default
- Chainable operations

### Agents
- Decision making
- Tool orchestration
- Memory management
- Error recovery

### Teams
- Agent collaboration
- Task distribution
- Progress tracking
- Resource sharing

### Pipelines
- Sequential workflows
- Type validation
- Performance monitoring
- Error propagation

## Features

- **Memory System**: Short-term, long-term, and episodic memory for agents
- **Metrics Tracking**: Built-in performance and usage metrics
- **Type Safety**: Full TypeScript support
- **Error Handling**: Comprehensive error management
- **Streaming**: Support for streaming responses
- **Caching**: Intelligent caching system

## Project Structure

```
project/
├── src/
│   ├── agents/     # Agent definitions
│   ├── tools/      # Tool implementations
│   ├── teams/      # Team configurations
│   ├── pipelines/  # Pipeline definitions
│   └── index.ts    # Main entry point
└── package.json
```

## Example

```typescript
import { symphony } from "symphonic";

// Create a tool
const analysisTool = symphony.tools.create({
    name: "analyze",
    description: "Analyze data",
    inputs: ["data"],
    handler: async (data) => {
        const analysis = await performAnalysis(data);
        return {
            success: true,
            result: analysis
        };
    }
});

// Create an agent
const analyst = symphony.agent.create({
    name: "analyst",
    description: "Data analyst",
    task: "Analyze complex datasets",
    tools: [analysisTool],
    llm: "gpt-4"
});

// Create a team
const analyticsTeam = symphony.team.create({
    name: "analytics",
    description: "Analytics team",
    agents: [analyst],
    manager: true
});

// Create a pipeline
const analyticsPipeline = symphony.pipeline.create({
    name: "analysis-pipeline",
    description: "Data analysis workflow",
    steps: [
        {
            name: "analyze",
            tool: analysisTool,
            description: "Perform analysis",
            chained: 1
        }
    ]
});

// Use the pipeline
const result = await analyticsPipeline.run({ data: dataset });
```

## Documentation

For detailed documentation and advanced usage, see [TESTUSAGE.md](./TESTUSAGE.md).

## License

MIT 
