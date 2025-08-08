# AI Town: Interactive AI Agent Simulation

## Project Overview
AI Town is a virtual town where AI characters live, chat, and socialize in real-time. It's a deployable starter kit for building and customizing interactive AI agent simulations, inspired by the research paper "Generative Agents: Interactive Simulacra of Human Behavior" by Park et al. (2023).

## Core Purpose
- **Interactive Simulation Platform**: Provides a real-time multiplayer environment where humans can directly interact with AI agents
- **Game-like Social Experience**: AI agents exhibit believable behaviors while allowing human players to join and participate
- **Extensible Framework**: Built with a strong foundation meant to be extended from simple projects to scalable multiplayer games
- **JavaScript/TypeScript Ecosystem**: Makes AI agent simulation accessible to web developers (vs Python-only implementations)

## Key Features
- **Real-time Multiplayer**: Humans can join the simulation and interact directly with AI agents
- **Visual 2D Environment**: PixiJS-powered game world with smooth character movement and animations
- **Conversation System**: AI agents can start conversations with each other and with human players
- **Memory & Personality**: Each agent has persistent memory, personality traits, and behavioral patterns
- **Scalable Architecture**: Built on Convex backend for real-time synchronization and data persistence
- **Customizable Characters**: Easy character creation with sprite sheets and personality definitions

## Research Applications
- Human-AI interaction studies in gaming contexts
- Real-time social simulation research
- Multiplayer AI behavior analysis
- Interactive storytelling and narrative generation
- Game AI development and testing

## Repository Structure
- `convex/`: Backend game engine and AI agent logic
- `src/`: React frontend with PixiJS game rendering
- `data/`: Character definitions, maps, and game assets
- `public/`: Static assets and sprite sheets
- `fly/`: Deployment configurations

## Technology Stack
- **Backend**: Convex (database, real-time sync, serverless functions)
- **Frontend**: React + TypeScript + PixiJS
- **AI**: Configurable LLM support (Ollama, OpenAI, Together.ai)
- **Deployment**: Vercel (frontend) + Convex Cloud (backend)
