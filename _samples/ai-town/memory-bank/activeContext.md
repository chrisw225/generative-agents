# AI Town Active Context

## Current Work Focus

AI Town is a **production-ready reference implementation** for interactive AI agent simulations. The project serves as both a playable experience and a framework for developers to build their own AI agent applications.

## Recent Changes & Current State

### Core Features Implemented
- **Real-time Multiplayer**: Humans can join and interact with AI agents seamlessly
- **Visual Game World**: 2D top-down view with smooth character movement and animations
- **AI Agent Behaviors**: Agents wander, initiate conversations, and respond to interactions
- **Memory System**: Vector-based memory storage for persistent agent experiences
- **Conversation System**: Natural language conversations between agents and humans
- **Customizable Characters**: Easy character creation with personalities and sprite sheets

### Character Roster (Current)
- **Lucky**: Happy, curious, loves cheese, space adventurer
- **Bob**: Grumpy gardener who avoids people, secretly resents missing college
- **Stella**: Charming sociopath who tricks people for money
- **Alice**: Brilliant scientist who speaks in riddles and appears forgetful
- **Pete**: Deeply religious, tries to convert everyone

## Next Steps & Active Decisions

### Immediate Development Priorities
1. **Performance Optimization**: Reduce input latency from current ~1.5s
2. **Mobile Experience**: Improve touch interactions and responsive design
3. **Character Diversity**: Add more varied personality types and behaviors
4. **Memory Enhancement**: Improve long-term memory persistence and retrieval

### Active Technical Considerations
- **Scaling Strategy**: Current single-threaded approach limits concurrent users
- **LLM Cost Management**: Balance response quality with API costs
- **State Management**: Optimize game state size for better performance
- **Error Handling**: Improve resilience to LLM failures and network issues

## Important Patterns & Preferences

### Architecture Decisions
- **Convex-First**: Leverage Convex platform capabilities fully rather than fighting them
- **Type Safety**: Maintain end-to-end TypeScript type safety
- **Real-time Priority**: Prioritize responsive user experience over perfect simulation
- **Extensibility**: Design for easy customization and extension by developers

### Code Organization Principles
- **Separation of Concerns**: Game engine, AI agents, and UI are cleanly separated
- **Input-Driven**: All state changes go through validated input handlers
- **Async Operations**: Long-running AI operations don't block the game loop
- **Historical Tracking**: Smooth interpolation for real-time movement

### AI Agent Design Philosophy
- **Believable over Perfect**: Agents should feel authentic rather than optimal
- **Personality-Driven**: Each agent has distinct traits that drive behavior
- **Memory-Informed**: Past interactions influence future behavior
- **Social Awareness**: Agents notice and respond to their environment

## Learnings & Project Insights

### What Works Well
- **Convex Integration**: Real-time sync and serverless functions provide excellent developer experience
- **PixiJS Performance**: Smooth 60 FPS rendering even with multiple characters
- **Input System**: Type-safe input validation prevents many runtime errors
- **Agent Operations**: Async LLM calls don't block the simulation

### Current Limitations
- **Input Latency**: 1.5s delay makes interactions feel sluggish
- **Memory Constraints**: In-memory game state limits world complexity
- **Single World**: No support for multiple concurrent worlds
- **Mobile UX**: Touch interactions need improvement

### Key Technical Insights
- **Historical Objects**: Essential for smooth movement with batched updates
- **Separation of Chat**: Keeping messages separate from game state improves performance
- **Vector Memory**: Embedding-based memory retrieval works well for agent personalities
- **Tick-Step Pattern**: High-frequency ticks with low-frequency saves balances performance

### Development Workflow Insights
- **Hot Reload**: Convex dev server enables rapid iteration on backend logic
- **Type Safety**: End-to-end TypeScript catches many issues early
- **Component Architecture**: React + PixiJS integration works smoothly
- **Local LLM**: Ollama enables development without API costs

## Integration Opportunities

### Potential Enhancements
- **Voice Integration**: Add speech-to-text and text-to-speech capabilities
- **Advanced AI**: Integrate more sophisticated reasoning models
- **Persistent Worlds**: Save and restore world state across sessions
- **Analytics**: Track user engagement and agent behavior patterns

### Cross-Project Learning
- **Memory Systems**: Vector-based approach could enhance other AI projects
- **Real-time Architecture**: Convex patterns applicable to other multiplayer apps
- **Agent Frameworks**: Input-driven design could improve other simulations
- **UI Patterns**: PixiJS + React integration useful for other game projects

## Current Challenges

### Technical Challenges
- **Latency Optimization**: Reducing input delay while maintaining consistency
- **Scalability**: Supporting more concurrent users and larger worlds
- **Cost Management**: Balancing LLM usage with operational costs
- **Error Recovery**: Graceful handling of AI service failures

### Design Challenges
- **Agent Authenticity**: Making AI behavior feel natural and engaging
- **User Onboarding**: Helping new users understand how to interact
- **Content Moderation**: Ensuring appropriate AI-generated content
- **Engagement Retention**: Keeping users interested over time

### Operational Challenges
- **Deployment Complexity**: Managing multiple services and dependencies
- **Monitoring**: Understanding system health and user experience
- **Version Management**: Coordinating frontend and backend updates
- **Community Building**: Growing developer and user communities

## Success Metrics Being Tracked

### User Engagement
- **Session Duration**: Average time users spend in the simulation
- **Return Rate**: Percentage of users who return for multiple sessions
- **Interaction Depth**: Quality and length of conversations with agents

### Technical Performance
- **Response Latency**: Time from user action to visible response
- **System Uptime**: Availability and reliability of the service
- **Concurrent Users**: Maximum simultaneous users supported

### Developer Adoption
- **Fork Activity**: Number of developers creating their own versions
- **Community Contributions**: Pull requests and issue engagement
- **Documentation Usage**: Access patterns for guides and examples
