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

#### (`src/tools/`)
```typescript
import { symphony } from 'symphonic';

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
  Tools are the fundamental units of work in Symphonic. They are pure functions that perform specific tasks with well-defined inputs and outputs.
  
  - Single responsibility: Each tool does one thing well
  - Pure functions: Same inputs always produce same outputs
  - Error handling: Always return `{ result, success }` objects
  - Async by default: All handlers are async functions

#### (`src/agents/`)
```typescript
import { symphony } from 'symphonic';

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
  Agents wrap tools with AI capabilities, using LLMs to:
  
  - Natural language interface
  - Automatic tool selection
  - Context awareness
  - Error recovery
  - Task decomposition

#### (`src/teams/`)
```typescript
import { symphony } from 'symphonic';

const myTeam = symphony.team.create({
    name: "teamName",
    description: "team purpose",
    agents: [agent1, agent2],
    manager: true,           // Enable team coordination
    log: {                   // Logging configuration
        inputs: true,
        outputs: true
    }
});

// Usage
await myTeam.run("complex task description");
```
  Teams organize multiple agents into collaborative units with:
  
  - Task distribution
  - Resource sharing
  - Progress monitoring
  - Error propagation
  - Result aggregation

#### (`src/pipelines/`)
```typescript
import { symphony } from 'symphonic';

const myPipeline = symphony.pipeline.create({
    name: "pipelineName",
    description: "pipeline purpose",
    steps: [
        {
            name: "step1",
            tool: tool1,
            description: "step purpose",
            chained: 1,           // Step 1 in execution order
            expects: {            // Input type definitions
                param1: "string"
            },
            outputs: {           // Output type definitions
                result: "object"
            }
        }
        // Additional steps...
    ]
});

// Usage
await myPipeline.run({ initialInput: value });
```
  Pipelines define fixed sequences of operations with:
  
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


<br/>

**Happy Building!**