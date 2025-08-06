# System Patterns: Generative Agents Architecture

## High-Level Architecture

```mermaid
graph TB
    subgraph "Frontend Layer"
        A[Django Web Server]
        B[WebSocket/HTTP API]
        C[Real-time Visualization]
    end
    
    subgraph "Backend Layer"
        D[Reverie Simulation Server]
        E[Agent Management]
        F[Memory Systems]
        G[Planning Engine]
    end
    
    subgraph "Agent Layer"
        H[Persona Instances]
        I[Cognitive Modules]
        J[Memory Structures]
    end
    
    subgraph "Data Layer"
        K[Simulation Storage]
        L[Agent Memories]
        M[Environment State]
    end
    
    A --> B
    B --> D
    D --> E
    E --> H
    H --> I
    I --> J
    J --> F
    F --> G
    G --> E
    H --> K
    K --> L
    K --> M
```

## Core System Patterns

### 1. **Two-Server Architecture Pattern**
- **Frontend Server**: Django-based web interface for visualization
- **Backend Server**: Python simulation engine for agent logic
- **Communication**: File-based state sharing and HTTP endpoints

### 2. **Agent-Centric Design Pattern**
Each agent (Persona) operates as an autonomous entity with:
- **Identity**: Unique name, age, personality traits
- **Memory**: Three-tier memory system
- **Planning**: Daily and hourly planning capabilities
- **Perception**: Environmental awareness and social perception
- **Action**: Movement and interaction capabilities

### 3. **Memory-Driven Behavior Pattern**

```mermaid
graph LR
    A[Perceive Environment] --> B[Retrieve Relevant Memories]
    B --> C[Generate Plans]
    C --> D[Execute Actions]
    D --> E[Store New Memories]
    E --> F[Reflect & Learn]
    F --> A
```

### 4. **Hierarchical Planning Pattern**
- **Daily Planning**: High-level daily objectives
- **Hourly Planning**: Time-blocked activities
- **Task Decomposition**: Breaking down complex activities
- **Action Execution**: Concrete movement and interaction

## Memory System Architecture

### Three-Tier Memory Structure

```mermaid
graph TD
    subgraph "Long-term Memory"
        A[Associative Memory<br/>Memory Stream]
        B[Spatial Memory<br/>Location Awareness]
    end
    
    subgraph "Short-term Memory"
        C[Scratch Space<br/>Current State]
    end
    
    subgraph "Memory Contents"
        A --> D[Events]
        A --> E[Thoughts]
        A --> F[Conversations]
        B --> G[Location Trees]
        B --> H[Object Relationships]
        C --> I[Current Action]
        C --> J[Daily Plan]
        C --> K[Social State]
    end
```

### Memory Retrieval Pattern
1. **Keyword-based**: Fast lookup using keyword indices
2. **Embedding-based**: Semantic similarity search
3. **Recency-weighted**: Recent memories prioritized
4. **Importance-weighted**: Significant memories prioritized

## Agent Decision-Making Flow

```mermaid
sequenceDiagram
    participant Env as Environment
    participant Agent as Persona Agent
    participant Memory as Memory System
    participant Planner as Planning Engine
    participant Action as Action Executor
    
    Env->>Agent: Current state & nearby events
    Agent->>Memory: Perceive & filter events
    Memory->>Agent: Relevant memories
    Agent->>Planner: Current state + memories
    Planner->>Planner: Generate/Update plan
    Planner->>Action: Next action details
    Action->>Env: Execute movement/interaction
    Agent->>Memory: Store new experience
```

## Communication Patterns

### Inter-Agent Communication
- **Direct Conversation**: When agents are co-located
- **Memory Influence**: Shared experiences affect future behaviors
- **Social Network**: Relationships form through repeated interactions

### Server Communication
- **State Synchronization**: Backend writes state files, frontend reads
- **Command Interface**: CLI-based simulation control
- **Real-time Updates**: WebSocket-like polling for visualization

## Data Flow Patterns

### Simulation State Flow
```mermaid
graph LR
    A[Base Simulation] --> B[Fork New Simulation]
    B --> C[Initialize Agents]
    C --> D[Load Memories]
    D --> E[Begin Simulation Loop]
    E --> F[Save State]
    F --> G[Compress for Demo]
```

### Memory Data Flow
```mermaid
graph TD
    A[New Experience] --> B[Create ConceptNode]
    B --> C[Store in Associative Memory]
    C --> D[Index by Keywords]
    C --> E[Generate Embedding]
    C --> F[Update Keyword Strength]
    D --> G[Fast Retrieval]
    E --> H[Semantic Search]
    F --> I[Importance Scoring]
```

## Scalability Patterns

### Agent Management
- **Lazy Loading**: Agents initialized on-demand
- **Memory Optimization**: Compressed storage for long-term memories
- **Parallel Processing**: Independent agent decision-making
- **State Checkpointing**: Regular saves for crash recovery

### Performance Optimization
- **Caching**: Keyword and embedding caches
- **Batch Processing**: Group operations where possible
- **Selective Attention**: Limited perception bandwidth
- **Memory Pruning**: Forgetting less important information

## Error Handling Patterns

### Graceful Degradation
- **API Failures**: Fallback to cached responses
- **Memory Corruption**: Bootstrap from base simulation
- **Network Issues**: Local file-based state management
- **Resource Limits**: Automatic memory cleanup

### Recovery Mechanisms
- **Checkpoint System**: Regular state snapshots
- **Incremental Saves**: Minimize data loss
- **Validation**: Memory integrity checks
- **Rollback**: Revert to last known good state
