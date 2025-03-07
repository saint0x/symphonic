# Symphonic SDK

Symphonic is the easiest way to build self-learning tools, agents, teams, and pipelines in minutes.

## Getting Started

1. Install the SDK:
```bash
npm install symphonic
# or
bun add symphonic
```

2. Import core functionality:
```typescript
import { symphony } from "symphonic";
```

3. Create your first tool:
```typescript
const myTool = symphony.tools.create({
    name: "example",
    description: "does something useful",
    inputs: ["data"],
    handler: async (data) => {
        return { result: data, success: true };
    }
});
```

4. Build up from there!

For a more thorough overview of functionality and detailed examples, check out our [Usage Guide](./USAGE.md).

## Core Concepts

### Tools: The Basic Building Blocks

Tools are the fundamental units of work in Symphonic. They are pure functions that perform specific tasks with well-defined inputs and outputs.

```typescript
const myTool = symphony.tools.create({
    name: "toolName",          // Unique identifier
    description: "what it does", // Used by agents for tool selection
    inputs: ["param1", "param2"], // Expected input parameters
    handler: async (param1, param2) => {
        // Your logic here
        return {
            result: returnedOutput,  // The function's expected output
            success: true          // Operation status
        };
    }
});

// Usage
await myTool.run({ param1: value1, param2: value2 });
```

Tools follow these principles:
- Single responsibility: Each tool does one thing well
- Pure functions: Same inputs always produce same outputs
- Error handling: Always return `{ result, success }` objects
- Async by default: All handlers are async functions

### Agents: Intelligent Tool Users

Agents wrap tools with AI capabilities, using LLMs to:
- Select appropriate tools for tasks
- Process inputs and outputs
- Handle error conditions
- Make decisions based on context

```typescript
const myAgent = symphony.agent.create({
    name: "agentName",
    description: "agent purpose",
    task: "specific task description",
    tools: [tool1, tool2],    // Tools this agent can use
    llm: "gpt-4"              // Language model to use
});

// Usage
await myAgent.run("natural language task description");
```

Agent features:
- Natural language interface
- Automatic tool selection
- Context awareness
- Error recovery
- Task decomposition

### Teams: Coordinated Agent Groups

Teams organize multiple agents into collaborative units with:
- Shared context
- Coordinated workflows
- Managed communication
- Centralized logging

```typescript
const myTeam = symphony.team.create({
    name: "teamName",
    description: "team purpose",
    agents: [agent1, agent2],
    manager: true,           // Enable team coordination -- if false, all agents return their own data 
    log: {                   // Logging configuration
        inputs: true,
        outputs: true
    }
});

// Usage
await myTeam.run("complex task description");
```

Team capabilities:
- Task distribution
- Resource sharing
- Progress monitoring
- Error propagation
- Result aggregation

### Pipelines: Fixed Workflows

Pipelines define fixed sequences of operations with:
- Explicit data flow
- Type checking
- Chain validation
- Performance optimization

```typescript
import { symphony } from 'symphonic';

const researchPipeline = symphony.pipeline.create({
    name: "researchPipeline",
    description: "Processes research in a structured way",
    steps: [
        {
            name: "search",
            tool: searchTool,
            description: "Perform initial research",
            chained: 1,
            expects: { query: "string" },
            outputs: { result: "object" }
        }
        // Additional steps...
    ]
});
```

Pipeline features:
- Static validation
- Performance optimization
- Error recovery
- Progress tracking
- Type safety

The SDK will validate this structure during initialization and provide clear error messages if any required components are missing or incorrectly structured.

## Project Structure

Symphonic enforces an opinionated project structure to ensure consistency and enable powerful tooling capabilities. Your project must follow this structure:

```
project/
├── src/
│   ├── agents/        # Agent definitions and behaviors
│   │   └── *.ts      # Each agent in a separate file
│   ├── tools/        # Tool implementations
│   │   └── *.ts      # Each tool in a separate file
│   ├── teams/        # Team compositions for agent collaboration
│   │   └── *.ts      # Each team in a separate file
│   ├── pipelines/    # Pipeline definitions for workflow automation
│   │   └── *.ts      # Each pipeline in a separate file
│   └── index.ts      # Main entry point and SDK initialization
└── package.json      # Project configuration
```

### Required Components

#### Agents (`src/agents/`)
```typescript
import { symphony } from 'symphonic';

const researchAgent = symphony.agent.create({
    name: "researchAgent",
    description: "Conducts research tasks",
    task: "perform research and analysis",
    tools: ["webSearch", "documentAnalysis"],
    llm: "gpt-4"
});
```

#### Tools (`src/tools/`)
```typescript
import { symphony } from 'symphonic';

const searchTool = symphony.tools.create({
    name: "searchTool",
    description: "Performs web searches",
    inputs: ["query"],
    handler: async (params) => {
        // Tool logic here
        return { success: true, result: "search results" };
    }
});
```

#### Teams (`src/teams/`)
```typescript
import { symphony } from 'symphonic';

const researchTeam = symphony.team.create({
    name: "researchTeam",
    description: "Collaborates on research tasks",
    agents: [researchAgent, writerAgent],
    manager: true,
    log: { inputs: true, outputs: true }
});
```

#### Pipelines (`src/pipelines/`)
```typescript
import { symphony } from 'symphonic';

const researchPipeline = symphony.pipeline.create({
    name: "researchPipeline",
    description: "Processes research in a structured way",
    steps: [
        {
            name: "search",
            tool: searchTool,
            description: "Perform initial research",
            chained: 1,
            expects: { query: "string" },
            outputs: { result: "object" }
        }
        // Additional steps...
    ]
});
```

The SDK will validate this structure during initialization and provide clear error messages if any required components are missing or incorrectly structured.

## Component Hierarchy

The components form a natural hierarchy:
1. Tools: Basic operations
2. Agents: Intelligent tool users
3. Teams: Coordinated agent groups
4. Pipelines: Fixed workflows

Each layer adds capabilities:
- Tools → Pure functions
- Agents → Intelligence
- Teams → Coordination
- Pipelines → Structure

## Best Practices

### Tool Design
- Keep tools simple and focused
- Use clear naming conventions
- Document inputs and outputs
- Handle errors gracefully
- Include type definitions

### Agent Configuration
- Provide clear task descriptions
- Choose appropriate LLMs
- Limit tool access appropriately
- Configure logging
- Handle timeouts

### Team Organization
- Group related agents
- Enable appropriate logging
- Configure management level
- Set communication patterns
- Define error policies

### Pipeline Construction
- Validate data flow
- Define types clearly
- Order steps logically
- Handle edge cases
- Monitor performance



Happy Building!