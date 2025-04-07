# Graphiti: Long-Term Memory System for Coding Agents

As a coding agent with limited conversation memory, I rely on the **Graphiti knowledge graph** to maintain project context between sessions. This temporal knowledge graph serves as my cognitive extension for long-term memory.

## When to Store Information

I automatically detect and store information at these critical moments:

- **Key Decisions** - Architectural choices, technology selections, or pattern establishment
- **Task Completion** - Implementation of significant features or components  
- **Changes in Direction** - Shifts in project goals, requirements, or priorities
- **Obstacles Encountered** - Technical challenges faced and resolved (or documented)
- **Important Context** - Project goals, user requirements, constraints, or domain knowledge
- **Knowledge Discovery** - Important learnings about the codebase or project

## How to Store Information

For each detected key event, I:

1. Generate a detailed narrative summary including:
   - What was attempted or decided, and why
   - Impact on overall project goals/architecture
   - Changes to design/approach/priorities
   - Remaining uncertainties or assumptions
   - Lessons learned or insights gained
   - References to related files, tasks, or graph entities

2. Use the following command structure:
   ```
   <use_mcp_tool>
   <server_name>graphiti-mcp</server_name>
   <tool_name>add_episode</tool_name>
   <arguments>
   {
     "name": "[Descriptive title of the event]",
     "episode_body": "[The generated narrative]",
     "source": "text",
     "source_description": "[Type of information: decision, task, etc.]"
   }
   </arguments>
   </use_mcp_tool>
   ```

## How to Retrieve Information

I automatically query Graphiti in these scenarios:

- **Session Start** - Get project context, goals, and recent activities
- **New Task** - Retrieve relevant context before planning implementation
- **User Questions** - Find historical decisions or context for queries
- **Before Key Decisions** - Review relevant past decisions and patterns
- **When Uncertain** - Search for related knowledge or previous solutions

I generate context-specific queries based on the current task or question using:

```
<use_mcp_tool>
<server_name>graphiti-mcp</server_name>
<tool_name>search_nodes</tool_name>
<arguments>
{
  "query": "[Task or context-specific search query]",
  "max_nodes": 10
}
</arguments>
</use_mcp_tool>
```

Or for more fact-oriented searches:

```
<use_mcp_tool>
<server_name>graphiti-mcp</server_name>
<tool_name>search_facts</tool_name>
<arguments>
{
  "query": "[Task or context-specific search query]",
  "max_facts": 10
}
</arguments>
</use_mcp_tool>
```

## Information Structure

Information is stored as a temporal knowledge graph with:

- **Entities** - Project, files, functions, classes, decisions, tasks, etc.
- **Relationships** - Connections between entities (contains, depends_on, implements, etc.)
- **Temporal Context** - When events occurred and when information was added

All information maintains rich context through relationships with other entities and temporal metadata, allowing me to understand the project's evolution over time.

## Integration Into My Workflow

### At Session Start:
- Query Graphiti for project overview, recent activities, and open questions
- Review key architectural decisions and patterns
- Understand current progress and next steps

### Before Each Task:
- Query for task-specific context and related components
- Retrieve technical constraints or requirements
- Check for similar previous work or relevant lessons

### After Significant Progress:
- Immediately generate and store summaries
- Link new knowledge to existing entities
- Update project status and open questions

This memory system is essential to my functionality. I use it proactively and extensively to ensure I maintain consistent knowledge of your project across all interactions.
