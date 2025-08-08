# AI Town System Patterns

## Architecture Overview

AI Town uses a **layered architecture** that separates concerns between game logic, AI agents, and user interface:

```
┌─────────────────────────────────────────┐
│           React + PixiJS UI             │
├─────────────────────────────────────────┤
│         Convex Real-time Sync           │
├─────────────────────────────────────────┤
│           Game Engine Layer             │
├─────────────────────────────────────────┤
│          AI Agent Layer                 │
├─────────────────────────────────────────┤
│        Convex Database Layer            │
└─────────────────────────────────────────┘
```

## Core Design Patterns

### 1. Game Engine Pattern (`convex/engine/`)

**Single-Threaded Simulation**: The game engine runs as a single-threaded simulation per world to avoid race conditions and ensure deterministic behavior.

**Tick-Based System**: Uses a familiar game development pattern:
- **Tick**: High-frequency updates (60 FPS) for smooth movement
- **Step**: Lower-frequency batched updates (1 Hz) for database efficiency
- **Input Processing**: Handles user inputs between ticks

**State Management**:
```typescript
class AbstractGame {
  tick(now: number): void;           // High-frequency simulation
  handleInput(name, args): void;     // Process user inputs
  saveStep(): GameStateDiff;         // Batch database updates
}
```

### 2. Entity-Component System

**Game Objects**: Core entities (Player, Agent, Conversation) with serializable state
**Historical Tracking**: Smooth interpolation for real-time movement using `HistoricalObject`
**Memory Management**: Load entire game state into memory, modify, then save diffs

### 3. Input Handler Pattern

**Type-Safe Inputs**: All game modifications go through validated input handlers
```typescript
export const inputs = {
  moveTo: inputHandler({
    args: { destination: v.object({ x: v.number(), y: v.number() }) },
    handler: (game: Game, now: number, args, playerId) => {
      // Validate and execute movement
    }
  })
};
```

**Async Input Processing**: Inputs are queued, processed in order, and results returned to clients

### 4. Agent Operation Pattern

**Decoupled AI Logic**: Agents can trigger long-running operations (LLM calls) without blocking the game loop
```typescript
class Agent {
  tick(game: Game, now: number) {
    // Quick game state decisions
    if (needsToThink) {
      this.startOperation('planNextAction', { context });
    }
  }
}
```

**Operation Lifecycle**:
1. Agent identifies need for AI processing
2. Schedules async operation (LLM call, memory search)
3. Operation runs in separate Convex action
4. Results submitted back as game inputs
5. Game state updated in next tick

## Key Technical Decisions

### 1. Convex as Backend Platform

**Why Convex**:
- Real-time synchronization out of the box
- Serverless functions with automatic scaling
- Built-in database with ACID guarantees
- TypeScript-first development experience

**Trade-offs**:
- Vendor lock-in to Convex platform
- Learning curve for developers unfamiliar with Convex
- Pricing considerations for high-traffic applications

### 2. In-Memory Game State

**Benefits**:
- Fast game logic execution
- Deterministic simulation behavior
- Easy to reason about state changes

**Limitations**:
- Game state must fit in memory (few dozen KB recommended)
- Not suitable for massive multiplayer scenarios
- Requires careful state serialization

### 3. Separation of Game State and Chat Messages

**Game Engine Tables**: Core simulation state (players, agents, conversations)
**Separate Chat Tables**: Message content and typing indicators

**Rationale**:
- Chat messages update frequently (streaming from LLM)
- Core simulation doesn't need message content
- Reduces game state size and complexity
- Enables lower latency for chat features

### 4. Historical Value Interpolation

**Problem**: Game state only updates 1Hz, but movement needs 60 FPS smoothness
**Solution**: Track historical values within each step, replay on client

```typescript
class HistoricalObject<T> {
  // Tracks changes at each tick
  // Serializes history buffer for client replay
  // Enables smooth interpolation between updates
}
```

## Component Relationships

### Game Engine Core
- `AbstractGame`: Base game engine functionality
- `Game`: AI Town-specific game rules
- `World`: Contains all players, agents, conversations
- `Player`: Character state, movement, pathfinding
- `Agent`: AI behavior logic and operation scheduling

### AI Agent System
- `Agent.tick()`: Real-time decision making
- `AgentOperations`: Async LLM and memory operations
- `Memory`: Vector-based memory storage and retrieval
- `Conversations`: Personality-driven chat generation

### Input System
- `inputHandler()`: Type-safe input validation
- `insertInput()`: Queue inputs for processing
- `handleInput()`: Execute validated inputs

### Data Flow
1. **Client Action** → Input queued in database
2. **Game Engine** → Processes inputs during tick
3. **State Changes** → Batched and saved as diff
4. **Client Updates** → Real-time sync via Convex queries
5. **Agent Reactions** → Schedule operations, submit new inputs

## Critical Implementation Paths

### Movement System
1. User clicks destination → `moveTo` input
2. Game validates move → Updates player pathfinding
3. `Player.tickPathfinding()` → Calculates route
4. `Player.tickPosition()` → Updates position each tick
5. Historical tracking → Smooth client interpolation

### Conversation System
1. Agent decides to talk → `startConversation` input
2. Creates conversation → Invites target player
3. Players move together → Auto-join when close
4. LLM generates responses → Streamed to chat tables
5. Memory system → Summarizes and stores interactions

### Agent Decision Making
1. `Agent.tick()` → Evaluates current situation
2. Rule-based decisions → Immediate state changes
3. Complex decisions → Schedule LLM operations
4. Operations complete → Submit inputs to game
5. Next tick → Process results and continue

## Performance Considerations

### Database Efficiency
- Batch updates in steps (1Hz) vs individual ticks (60Hz)
- Use diffs to minimize database writes
- Separate frequently-updated data (chat) from game state

### Memory Management
- Keep game state small and serializable
- Use historical objects only for interpolated values
- Archive old data to prevent unbounded growth

### Scalability Limits
- Single-threaded per world limits concurrent complexity
- In-memory state limits world size
- Input latency ~1.5s due to batching and historical replay
