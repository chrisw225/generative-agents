# AI Town Technical Context

## Technology Stack

### Backend: Convex Platform
- **Database**: NoSQL document store with ACID guarantees
- **Real-time Sync**: Automatic client synchronization via WebSocket
- **Serverless Functions**: TypeScript functions for queries, mutations, actions
- **Vector Search**: Built-in vector database for AI memory storage
- **Scheduling**: Cron jobs and function scheduling
- **File Storage**: Asset and image storage capabilities

### Frontend: React + PixiJS
- **React 18**: Component-based UI with hooks
- **TypeScript**: Type-safe development throughout
- **PixiJS 7**: High-performance 2D WebGL rendering
- **@pixi/react**: React bindings for PixiJS components
- **Tailwind CSS**: Utility-first styling framework
- **Vite**: Fast development server and build tool

### AI Integration
- **Ollama**: Local LLM inference (default: llama3)
- **OpenAI API**: Cloud-based LLM option
- **Together.ai**: Alternative cloud LLM provider
- **Vector Embeddings**: For agent memory and retrieval
- **Replicate**: Background music generation (optional)

### Development Tools
- **ESLint**: Code linting and style enforcement
- **Prettier**: Code formatting
- **Jest**: Unit testing framework
- **npm-run-all**: Parallel script execution
- **Concurrently**: Development server coordination

## Development Setup

### Prerequisites
- **Node.js 18+**: JavaScript runtime
- **npm**: Package manager
- **Convex Account**: Backend platform (free tier available)
- **LLM Provider**: Ollama (local) or cloud API keys

### Local Development Workflow
```bash
# Install dependencies
npm install

# Start development (frontend + backend)
npm run dev

# Or run separately
npm run dev:frontend  # Vite dev server on :5173
npm run dev:backend   # Convex dev server with hot reload
```

### Environment Configuration
```bash
# .env.local
VITE_CLERK_PUBLISHABLE_KEY=pk_***     # Optional auth
CLERK_SECRET_KEY=sk_***               # Optional auth
OPENAI_API_KEY=sk-***                 # If using OpenAI
TOGETHER_API_KEY=***                  # If using Together.ai
REPLICATE_API_TOKEN=***               # Optional music generation
```

## Technical Constraints

### Convex Platform Limitations
- **Function Timeout**: 10 minutes for actions, 1 minute for mutations
- **Memory Limits**: 512MB per function execution
- **Database Size**: Generous limits but not unlimited
- **Vendor Lock-in**: Convex-specific APIs and deployment

### Game Engine Constraints
- **Single-threaded**: One world per engine instance
- **Memory-bound**: Game state must fit in memory (~few dozen KB)
- **Input Latency**: ~1.5s due to batching and historical replay
- **Tick Rate**: 60 FPS ticks, 1 Hz database updates

### Browser Compatibility
- **WebGL Support**: Required for PixiJS rendering
- **Modern Browsers**: ES2020+ features used throughout
- **Mobile Support**: Touch interactions supported but not optimized

## Dependencies Analysis

### Core Dependencies
```json
{
  "convex": "^1.19.2",           // Backend platform
  "react": "18.2.0",             // UI framework
  "pixi.js": "^7.2.4",           // 2D rendering
  "@pixi/react": "^7.1.0",       // React-PixiJS integration
  "typescript": "5.1.3"          // Type safety
}
```

### AI/ML Dependencies
```json
{
  "hnswlib-node": "^1.4.2",      // Vector similarity search
  "replicate": "^0.18.0"         // AI model API client
}
```

### Development Dependencies
```json
{
  "vite": "^4.4.9",              // Build tool
  "tailwindcss": "^3.3.3",       // CSS framework
  "eslint": "8.42.0",            // Linting
  "@types/react": "18.2.8"       // TypeScript definitions
}
```

## Deployment Architecture

### Production Stack
- **Frontend**: Vercel (static site deployment)
- **Backend**: Convex Cloud (managed serverless)
- **Database**: Convex managed database
- **CDN**: Vercel Edge Network for assets
- **Monitoring**: Convex dashboard and logging

### Alternative Deployments
- **Docker**: Self-hosted Convex backend option
- **Fly.io**: Container deployment with custom setup
- **Local**: Full local development with Ollama

## Tool Usage Patterns

### Convex Development
```typescript
// Query (reactive data fetching)
export default function GameView() {
  const world = useQuery(api.game.main.worldStatus, { worldId });
  return <GameWorld world={world} />;
}

// Mutation (immediate data changes)
const sendInput = useMutation(api.game.main.sendInput);
await sendInput({ name: 'moveTo', args: { destination: { x, y } } });

// Action (long-running operations)
export const generateResponse = internalAction({
  handler: async (ctx, args) => {
    const response = await openai.chat.completions.create({...});
    await ctx.runMutation(api.messages.add, { response });
  }
});
```

### PixiJS Integration
```typescript
// React component with PixiJS
function GameCharacter({ player }: { player: Player }) {
  return (
    <Sprite
      texture={characterTexture}
      x={player.position.x}
      y={player.position.y}
      interactive
      pointerdown={() => startConversation(player.id)}
    />
  );
}
```

### Historical Value Interpolation
```typescript
// Smooth movement despite 1Hz updates
const historicalPosition = useHistoricalValue(
  player.locationHistory,
  currentTime
);
```

## Performance Optimization Strategies

### Client-Side
- **PixiJS Optimization**: Sprite batching, texture atlases
- **React Optimization**: useMemo, useCallback for expensive operations
- **Network Efficiency**: Convex handles query optimization automatically

### Server-Side
- **Batch Operations**: Group database writes in steps
- **Memory Management**: Serialize/deserialize game state efficiently
- **Vector Search**: Cache embeddings to avoid recomputation

### Database
- **Index Strategy**: Convex auto-indexes based on query patterns
- **Data Archival**: Move old conversations/memories to archive tables
- **Query Optimization**: Use Convex query planner insights

## Integration Points

### LLM Integration
- **Configurable Providers**: Support multiple LLM backends
- **Streaming Responses**: Real-time chat message updates
- **Error Handling**: Graceful fallbacks for LLM failures
- **Rate Limiting**: Respect API limits and costs

### Authentication (Optional)
- **Clerk Integration**: Social login, user management
- **Anonymous Play**: Allow users without accounts
- **Session Management**: Persistent user identity across visits

### Asset Pipeline
- **Sprite Sheets**: Character animations and textures
- **Map Data**: Tiled map editor integration
- **Audio**: Background music and sound effects
- **Image Generation**: AI-generated character portraits (optional)

## Security Considerations

### Client-Server Security
- **Input Validation**: All inputs validated on server
- **Rate Limiting**: Prevent spam and abuse
- **Authentication**: Optional but recommended for production

### AI Safety
- **Content Filtering**: Monitor AI-generated content
- **Prompt Injection**: Sanitize user inputs to LLMs
- **Cost Control**: Monitor and limit LLM API usage

### Data Privacy
- **User Data**: Minimal collection, clear retention policies
- **Conversation Logs**: Consider privacy implications
- **Analytics**: Respect user privacy preferences
