# Testing Ideas for Graphiti Temporal Memory Graph

This document outlines strategies, prompts, and workflows for testing the Graphiti Temporal Memory Graph as a memory layer for AI agents. The focus is on practical testing approaches to evaluate how effectively agents can store, retrieve, and utilize information from the knowledge graph.

## Core Testing Objectives

1. **Information Storage**: Test when and what information should be stored in the graph
2. **Information Retrieval**: Evaluate how effectively agents can retrieve relevant information
3. **Temporal Reasoning**: Assess the agent's ability to reason about information across time
4. **Knowledge Utilization**: Determine how well agents can apply retrieved knowledge

## Prompt Templates for Testing

### 1. Information Storage Prompts

These prompts help test the agent's ability to identify and store relevant information:

```
When you encounter new information about [topic/entity], store it in the knowledge graph using the add_episode tool. Focus on capturing:
1. The specific entities involved
2. Their relationships to existing entities
3. Any temporal aspects (when this information became valid)
4. The source of the information

Format the episode as [text/JSON] with a descriptive name that will help with future retrieval.
```

```
As we discuss this [codebase/project/topic], identify key concepts, components, and relationships that should be preserved for future reference. For each important piece of information:
1. Create an episode with a clear, searchable name
2. Include relevant context that would help future understanding
3. Tag it appropriately with a source description
```

### 2. Information Retrieval Prompts

These prompts test how effectively agents can retrieve information from the graph:

```
Before answering my question about [topic], search the knowledge graph for relevant information using both:
1. A semantic search with search_nodes using natural language: "[descriptive query]"
2. A fact-based search with search_facts focusing on relationships: "[relationship query]"

Combine the retrieved information with your general knowledge to provide a comprehensive answer.
```

```
I need information about [specific entity/concept]. Please:
1. Search for nodes related to this entity
2. Explore any connected facts or relationships
3. Check if there are temporal aspects to consider (has this changed over time?)
4. Synthesize what you find into a coherent explanation
```

### 3. Temporal Reasoning Prompts

These prompts test the agent's ability to reason about information across time:

```
How has [entity/concept] evolved over time? Search the knowledge graph for temporal information about this topic and construct a timeline of key changes or developments.
```

```
Compare the state of [entity/component] between [earlier time point] and [later time point]. What significant changes occurred, and what implications did these changes have?
```

### 4. Knowledge Application Prompts

These prompts test how well agents can apply retrieved knowledge:

```
Based on the information stored in the knowledge graph about [topic], what recommendations would you make for [specific task/decision]? Support your recommendations with specific facts from the graph.
```

```
Using the historical patterns stored in the knowledge graph, predict potential issues or challenges that might arise with [component/feature]. What preventative measures would you suggest?
```

## Testing Workflows

### Workflow 1: Progressive Knowledge Building

This workflow tests incremental knowledge accumulation and retrieval:

1. **Initial Information Storage**:
   - Share a piece of information about a topic with the agent
   - Ask the agent to store this information in the graph
   - Verify the information was stored correctly using get_episodes

2. **Knowledge Expansion**:
   - Provide additional, related information that builds on the initial knowledge
   - Ask the agent to store this new information, relating it to existing knowledge
   - Verify the connections were made properly

3. **Retrieval Testing**:
   - Ask questions that require combining multiple pieces of stored information
   - Evaluate the completeness and accuracy of the retrieved information
   - Note any gaps or misunderstandings in the agent's knowledge

4. **Contradiction Handling**:
   - Introduce information that contradicts or updates previously stored knowledge
   - Observe how the agent handles this contradiction (should use temporal invalidation)
   - Verify that queries now return the updated information

### Workflow 2: Project Understanding Testing

This workflow tests how well the agent can build and utilize a knowledge graph of a software project:

1. **Project Ingestion**:
   - Share key components of a software project (architecture, modules, dependencies)
   - Ask the agent to create a structured representation in the graph
   - Verify the project structure is accurately captured

2. **Relationship Mapping**:
   - Describe relationships between components (X depends on Y, A calls B)
   - Have the agent store these relationships as facts
   - Test retrieval of these relationships with specific queries

3. **Evolution Tracking**:
   - Introduce changes to the project (new features, refactoring, bug fixes)
   - Ask the agent to update the knowledge graph accordingly
   - Test temporal queries about the state of components before and after changes

4. **Problem-Solving**:
   - Present a problem or question about the project
   - Ask the agent to use the knowledge graph to solve or answer it
   - Evaluate the relevance and accuracy of the retrieved information

### Workflow 3: Conversational Memory Testing

This workflow tests how well the agent can use the graph for maintaining conversation context:

1. **Conversation Capture**:
   - Have a multi-turn conversation about a complex topic
   - Ask the agent to store key points from the conversation as episodes
   - Verify the important information was captured

2. **Context Retrieval**:
   - After some time, resume the conversation with an ambiguous reference
   - Observe if the agent can retrieve the relevant context from the graph
   - Evaluate the continuity and coherence of the conversation

3. **Long-term Reference**:
   - Much later, ask about a specific detail mentioned in the earlier conversation
   - Test if the agent can retrieve this information accurately
   - Note the difference between graph-retrieved information and what might be in the agent's context window

## Specific Testing Scenarios

### Scenario 1: Code Understanding and Documentation

Test the agent's ability to build and utilize a knowledge graph of code:

1. Share code snippets from a project, asking the agent to store:
   - Functions and their purposes
   - Class hierarchies and relationships
   - Dependencies between components

2. Ask questions that require understanding the code structure:
   - "What functions call the X method?"
   - "Which components depend on module Y?"
   - "How has function Z changed over time?"

3. Request the agent to generate documentation based on the stored knowledge:
   - API documentation for specific functions
   - Architecture overview based on component relationships
   - Change logs based on temporal information

### Scenario 2: Bug Analysis and Resolution

Test the agent's ability to use the graph for debugging:

1. Store information about previous bugs and their resolutions
2. Present a new bug with similar characteristics
3. Ask the agent to:
   - Search for similar bugs in the knowledge graph
   - Analyze patterns and common factors
   - Suggest solutions based on historical resolutions
4. Evaluate how effectively the agent leverages past knowledge

### Scenario 3: Project Evolution Analysis

Test the agent's ability to reason about project changes over time:

1. Store information about multiple versions of a project
2. Ask the agent to analyze:
   - How specific components evolved
   - When certain features were introduced
   - The impact of architectural changes
3. Request predictions about future maintenance needs based on historical patterns

## Guidelines for Effective Testing

### When to Store Information

Train and test the agent to store information when:

1. **New entities** are introduced (components, concepts, requirements)
2. **Relationships** between entities are established or changed
3. **Important decisions** are made that affect the project
4. **Problems and solutions** are encountered
5. **Temporal events** occur (version releases, major changes)
6. **Contradictions** to previously stored information arise

### What Information to Store

Test the agent's ability to identify and store:

1. **Structured data** that fits defined entity types (Functions, Components, etc.)
2. **Relationships** between entities with clear predicates
3. **Temporal metadata** (when information became valid/invalid)
4. **Source context** (where the information came from)
5. **Searchable names** and descriptions that facilitate future retrieval

### Effective Retrieval Strategies

Test different retrieval approaches:

1. **Semantic search** for concept-based queries
2. **Keyword search** for specific identifiers
3. **Graph traversal** for relationship exploration
4. **Temporal queries** for historical analysis
5. **Hybrid approaches** combining multiple search methods

## Evaluation Metrics

Measure the effectiveness of the memory graph with these metrics:

1. **Retrieval accuracy**: How relevant is the retrieved information?
2. **Retrieval completeness**: Is all relevant information retrieved?
3. **Temporal accuracy**: Does the agent correctly understand when information was valid?
4. **Contradiction handling**: Does the agent properly manage updated information?
5. **Knowledge application**: Can the agent effectively use retrieved information?
6. **Storage efficiency**: Does the agent store information in a way that facilitates retrieval?

## Next Steps for Testing

1. Start with simple, controlled scenarios and gradually increase complexity
2. Compare agent performance with and without the knowledge graph
3. Identify patterns of effective prompting that maximize graph utilization
4. Develop automated testing scripts to evaluate retrieval effectiveness
5. Create benchmark datasets with known information to measure retrieval accuracy