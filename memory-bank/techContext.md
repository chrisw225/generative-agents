# Technical Context: Generative Agents

## Technology Stack

### Core Technologies
- **Python 3.9.12**: Primary language for simulation engine
- **Django 2.2**: Web framework for frontend visualization
- **OpenAI GPT**: Large language model for agent decision-making
- **SQLite**: Local database for simulation state
- **JavaScript**: Frontend visualization and interaction

### Key Dependencies
```python
# Core AI/ML
openai==0.27.0          # GPT integration
gensim==3.8.0           # Word embeddings
scikit-learn==1.3.0     # Machine learning utilities
nltk==3.6.5            # Natural language processing

# Web Framework
Django==2.2            # Web application framework
django-cors-headers==2.5.3  # CORS support

# Data Processing
pandas==2.0.3          # Data manipulation
numpy==1.25.2          # Numerical computing
matplotlib==3.7.2      # Visualization
seaborn==0.12.2        # Statistical visualization

# Utilities
python-dateutil==2.8.2  # Date/time handling
psycopg2-binary==2.9.5  # Database connectivity
```

## System Architecture

### Backend Architecture (reverie/)
```
reverie/
├── backend_server/
│   ├── reverie.py              # Main simulation server
│   ├── persona/
│   │   ├── persona.py          # Core agent class
│   │   ├── cognitive_modules/  # Decision-making modules
│   │   ├── memory_structures/  # Memory implementations
│   │   └── prompt_template/    # GPT prompt templates
│   ├── maze.py                 # Environment representation
│   └── path_finder.py          # Navigation algorithms
```

### Frontend Architecture (environment/)
```
environment/frontend_server/
├── manage.py                   # Django management
├── frontend_server/
│   ├── settings/              # Django configuration
│   ├── urls.py                # URL routing
│   └── wsgi.py               # WSGI application
├── translator/                # API endpoints
├── templates/                 # HTML templates
├── static_dirs/              # Static assets
├── storage/                  # Simulation data
└── compressed_storage/       # Demo-ready simulations
```

## Memory System Implementation

### Associative Memory (Memory Stream)
- **Storage**: JSON files with embeddings
- **Structure**: ConceptNode objects with metadata
- **Indexing**: Keyword-based and embedding-based
- **Retrieval**: Recency + relevance scoring

### Spatial Memory
- **Format**: Tree structure for location hierarchy
- **Navigation**: Path-finding with A* algorithm
- **Updates**: Dynamic location discovery

### Scratch Memory
- **Purpose**: Current state and short-term planning
- **Persistence**: JSON serialization for save/load
- **Scope**: Single simulation session

## API Design

### Backend API (CLI-based)
```bash
# Start simulation
python reverie.py

# Commands available:
run <steps>          # Run simulation for N steps
fin                  # Save and exit
exit                 # Exit without saving
call -- load history # Load agent history
```

### Frontend API (HTTP-based)
```
GET /                 # Main visualization
GET /simulator_home   # Simulation control
GET /replay/<sim>/<step>  # Replay simulation
GET /demo/<sim>/<step>/<speed>  # Demo mode
```

## Data Models

### Agent State Model
```python
class Persona:
    name: str                    # Unique identifier
    s_mem: SpatialMemory        # Location awareness
    a_mem: AssociativeMemory    # Long-term memory
    scratch: Scratch           # Current state
    
    # Planning state
    daily_plan: List[str]      # Daily objectives
    hourly_schedule: List[tuple]  # Time-blocked activities
    current_action: dict       # Active action details
```

### Memory Node Model
```python
class ConceptNode:
    node_id: str               # Unique identifier
    node_type: str             # event/thought/chat
    created: datetime          # Creation timestamp
    description: str           # Natural language description
    embedding: List[float]     # Semantic embedding
    keywords: Set[str]         # Indexed keywords
    poignancy: int             # Importance score
```

## Environment Configuration

### Development Setup
```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Configure OpenAI API
# Create reverie/backend_server/utils.py:
openai_api_key = "your-api-key"
key_owner = "your-name"

# 3. Start frontend
cd environment/frontend_server
python manage.py runserver

# 4. Start backend (new terminal)
cd reverie/backend_server
python reverie.py
```

### Environment Variables
```python
# maze_assets_loc: Path to environment assets
# fs_storage: Simulation storage location
# fs_temp_storage: Temporary storage for current state
# collision_block_id: Environment collision settings
```

## Performance Characteristics

### Resource Requirements
- **Memory**: 2-4GB RAM for 25 agents
- **Storage**: 50-100MB per simulation hour
- **CPU**: Single-threaded, LLM-bound
- **Network**: Local file I/O, no external dependencies

### Bottlenecks
1. **OpenAI API Rate Limits**: Hourly token quotas
2. **Memory Growth**: Unbounded memory accumulation
3. **Storage I/O**: Frequent state serialization
4. **LLM Latency**: Decision-making delays

## Security Considerations

### API Key Management
- **Storage**: Local utils.py file (not in repo)
- **Access**: Single-user development setup
- **Rotation**: Manual key updates required

### Data Privacy
- **Local Storage**: All data stored locally
- **No External Sharing**: Research-only usage
- **Memory Sanitization**: No PII in agent memories

## Deployment Patterns

### Local Development
- **Single Machine**: All components on localhost
- **File-based**: Local storage for all data
- **Development Mode**: Debug enabled, hot reload

### Research Deployment
- **Isolated Environment**: Clean VM/container
- **Persistent Storage**: Mounted volumes for data
- **Monitoring**: Basic logging and metrics

### Demo Deployment
- **Compressed Storage**: Optimized for web delivery
- **Static Serving**: Pre-computed simulations
- **Read-only**: No live simulation capabilities

## Extension Points

### Adding New Agents
1. Create persona directory in storage
2. Define initial memories and traits
3. Configure spatial memory for locations
4. Test integration with existing agents

### Custom Behaviors
1. Extend cognitive modules
2. Add new prompt templates
3. Modify planning algorithms
4. Update memory structures

### New Environments
1. Create new map files (Tiled editor)
2. Define location hierarchies
3. Configure object relationships
4. Update navigation paths
