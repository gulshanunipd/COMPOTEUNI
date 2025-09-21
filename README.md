# COMPOTE - COntext-aware Message seed PriOritization and muTation in consEnsus fuzzing

A comprehensive implementation of the COMPOTE fuzzing engine for consensus protocol testing, based on the research paper algorithms with full modular architecture and plugin support.

## 🎯 Overview

COMPOTE is an advanced fuzzing engine specifically designed for consensus protocols that implements:

- **Algorithm 1**: Selective & Conditional Parsing (O(1) per message)
- **Algorithm 2**: Feature Extraction (O(M·f) complexity)  
- **Algorithm 3**: Context Clustering using DBSCAN (O(n·log n) average)
- **Algorithm 4**: Priority Calculation with weighted scoring (O(n·|T|) with pruning)
- **Algorithm 5**: State Analysis & Priority Update with feedback loops

## 🚀 Key Features

### Core Algorithms
- ✅ **Selective Parsing**: Efficiently extracts only critical fields from consensus messages
- ✅ **Feature Extraction**: Converts messages into numerical vectors for clustering
- ✅ **Context Clustering**: Groups similar messages using DBSCAN with Euclidean distance
- ✅ **Priority Calculation**: Weighted scoring combining similarity, fault history, and coverage
- ✅ **State Analysis**: Drives fuzzing iterations with continuous refinement

### Architecture
- 🔧 **Modular Design**: Each algorithm implemented as discrete, reusable component
- 🔌 **Plugin System**: Seamless integration with LOKI and Tyr frameworks
- ⚡ **Performance Optimized**: O(n·log n) overall complexity with caching and pruning
- 🎛️ **Configurable**: Extensive configuration options for all algorithms
- 📊 **Monitoring**: Real-time statistics and performance metrics

### Advanced Features
- 🤖 **Simulation Mode**: Test without real consensus networks
- 🔄 **Auto-optimization**: Dynamic parameter tuning based on performance
- 📈 **Coverage Tracking**: Code coverage and new path discovery
- 🐛 **Fault Detection**: Comprehensive fault categorization and analysis
- 💾 **State Persistence**: Save/load fuzzing campaigns
- 🎨 **Visualization**: Cluster analysis and performance plots

## 📦 Installation

### Requirements
```bash
# Python 3.7+
pip install -r requirements.txt
```

### Dependencies
- `numpy>=1.21.0` - Numerical computations
- `scikit-learn>=1.0.0` - Machine learning algorithms (DBSCAN)
- `pandas>=1.3.0` - Data manipulation
- `matplotlib>=3.5.0` - Visualization (optional)
- `pytest>=6.2.0` - Testing framework

### Quick Install
```bash
git clone <repository>
cd compote
pip install -r requirements.txt
```

## 🔧 Quick Start

### Basic Usage
```python
from compote import CompoteFuzzer
from compote.core.types import RawMessage
import json
import time

# Create fuzzer with configuration
config = {
    'simulation_mode': True,
    'max_iterations': 100,
    'clustering_eps': 0.3,
    'priority_alpha': 0.3,  # similarity weight
    'priority_beta': 0.4,   # fault weight  
    'priority_gamma': 0.3   # coverage weight
}

with CompoteFuzzer(config) as fuzzer:
    # Load consensus messages
    messages = [
        {
            'message_type': 'propose',
            'round_number': 1,
            'sender_id': 'leader_node',
            'block_hash': 'block_123'
        }
        # ... more messages
    ]
    
    fuzzer.load_messages(messages)
    fuzzer.initialize_seed_pool()
    
    # Start fuzzing
    fuzzer.start_fuzzing()
    
    # Get results
    report = fuzzer.get_comprehensive_report()
    print(f"Faults found: {report['summary']['faults_discovered']}")
```

### Plugin Integration
```python
from compote.plugins import LokiPlugin, TyrPlugin
from compote.fabric import FabricPlugin

# LOKI integration
loki_plugin = LokiPlugin({
    'loki_host': 'localhost',
    'loki_port': 9001,
    'seed_batch_size': 10
})

# Tyr integration  
tyr_plugin = TyrPlugin({
    'tyr_path': '/usr/local/bin/tyr',
    'working_dir': '/tmp/compote_tyr'
})

# Hyperledger Fabric v2.5 integration
fabric_plugin = FabricPlugin({
    'fabric_version': '2.5',
    'network_name': 'compote-fabric',
    'rest_api_base_url': 'http://localhost:4000',
    'orderer_endpoints': ['localhost:7050'],
    'peer_endpoints': ['localhost:7051', 'localhost:8051']
})
```

## 📖 Examples

### Run Basic Example
```bash
python examples/basic_usage.py
```

### Advanced Features Demo
```bash
python examples/advanced_usage.py
```

### Performance Benchmarking
```bash
python examples/performance_benchmark.py
```

## 🧪 Testing

### Run All Tests
```bash
python tests/run_tests.py
```

### Coverage Analysis
```bash
python tests/run_tests.py --coverage
```

### Performance Tests
```bash
python tests/run_tests.py --performance
```

### Specific Test Patterns
```bash
python tests/run_tests.py --pattern "test_parser*"
```

## 📊 Algorithm Implementation Details

### Algorithm 1: Selective & Conditional Parsing
- **Purpose**: Extract critical fields from raw consensus messages
- **Complexity**: O(1) per message
- **Implementation**: Two-stage parsing with common fields (selective) and type-specific fields (conditional)
- **Formats Supported**: JSON, Binary, Protobuf, Custom

### Algorithm 2: Feature Extraction  
- **Purpose**: Convert parsed messages to numerical vectors
- **Complexity**: O(M·f) for M messages and f features
- **Features**: 
  - Categorical (message type, sender role)
  - Numerical (round, view, height)
  - Temporal (latency, deviation)
  - Structural (entropy, complexity)
  - Derived (role weights, distances)

### Algorithm 3: Context Clustering
- **Purpose**: Group similar messages into context pools
- **Algorithm**: DBSCAN with Euclidean distance
- **Complexity**: O(n·log n) average case
- **Features**: Parameter optimization, silhouette scoring, visualization

### Algorithm 4: Priority Calculation
- **Purpose**: Assign priority scores within clusters
- **Formula**: P = α·similarity + β·fault + γ·coverage
- **Complexity**: O(n·|T|) with threshold-based pruning
- **Features**: Time decay, execution weighting, cache optimization

### Algorithm 5: State Analysis & Priority Update
- **Purpose**: Drive fuzzing iterations with feedback
- **Components**: 
  - Message selection and mutation
  - Execution and result analysis
  - Priority queue updates
  - Continuous refinement

## 🔌 Plugin System

### LOKI Integration
- Real-time message seeding
- Execution feedback collection
- Coverage data sharing
- Auto-reseeding capabilities

### Tyr Integration  
- Batch message processing
- Process-based communication
- Result monitoring
- Performance optimization

### Custom Plugins
Extend the `PluginInterface` to create custom integrations:

```python
from compote.plugins.base_plugin import PluginInterface

class CustomPlugin(PluginInterface):
    def initialize(self) -> bool:
        # Plugin initialization
        pass
    
    def provide_seed_messages(self, count: int) -> List[ParsedMessage]:
        # Provide high-priority seeds
        pass
    
    def execute_message(self, message: ParsedMessage) -> ExecutionResult:
        # Execute message on target
        pass
    
    def receive_feedback(self, result: ExecutionResult) -> bool:
        # Process execution feedback
        pass
```

## ⚙️ Configuration

### Core Configuration
```python
config = {
    # Feature extraction
    'normalize_features': True,
    
    # Clustering  
    'clustering_eps': 0.5,
    'clustering_min_samples': 3,
    'scale_features': True,
    
    # Priority calculation
    'priority_alpha': 0.3,  # similarity weight
    'priority_beta': 0.4,   # fault weight
    'priority_gamma': 0.3,  # coverage weight
    'fault_threshold': 0.1,
    'similarity_threshold': 0.2,
    'time_threshold': 3600.0,
    
    # Mutation
    'timestamp_variance': 0.1,
    'round_variance': 2,
    'view_variance': 1,
    'enabled_mutations': ['timestamp', 'round', 'view', 'payload'],
    
    # Execution
    'simulation_mode': False,
    'execution_timeout': 30.0,
    'max_retries': 3,
    
    # Performance
    'max_workers': 4,
    'max_iterations': 1000,
    'save_interval': 100,
    'auto_optimize': True
}
```

## 📈 Performance

### Benchmarks (on standard hardware)
- **Parsing**: >1000 messages/second
- **Feature Extraction**: >500 messages/second  
- **Clustering**: <5 seconds for 1000 messages
- **Priority Calculation**: <2 seconds for 1000 messages
- **End-to-End**: >20 iterations/second

### Scalability
- Linear scaling with message count
- O(n·log n) overall complexity
- Memory-efficient with pruning
- Concurrent execution support

## 🛠️ Development

### Project Structure
```
compote/
├── compote/           # Main package
│   ├── core/          # Core algorithms
│   ├── engine/        # Fuzzing engine
│   ├── plugins/       # Plugin system
│   └── utils/         # Utilities
├── examples/          # Usage examples
├── tests/             # Unit tests
└── docs/              # Documentation
```

### Contributing
1. Fork the repository
2. Create feature branch
3. Add tests for new functionality
4. Ensure all tests pass
5. Submit pull request

### Code Style
- PEP 8 compliant
- Type hints for all public APIs
- Comprehensive docstrings
- 90%+ test coverage

## 📚 Documentation

- [API Reference](docs/api.md) - Detailed API documentation
- [Architecture Guide](docs/architecture.md) - System design overview
- [Algorithm Details](docs/algorithms.md) - In-depth algorithm explanations
- [Plugin Development](docs/plugins.md) - Creating custom plugins
- [Performance Tuning](docs/performance.md) - Optimization guidelines

## 🎯 Use Cases

### Consensus Protocol Testing
- **Blockchain Networks**: Bitcoin, Ethereum, Hyperledger
- **Distributed Systems**: Raft, PBFT, HotStuff
- **Message Protocols**: Tendermint, Algorand, Avalanche

### Integration Scenarios
- **CI/CD Testing**: Automated consensus testing
- **Security Audits**: Vulnerability discovery
- **Performance Analysis**: Throughput and latency testing
- **Regression Testing**: Protocol upgrade validation

## 🔒 Security Considerations

- **Simulation Mode**: Safe testing without network impact
- **Input Validation**: Comprehensive message sanitization
- **Resource Limits**: Configurable execution timeouts and limits
- **Isolation**: Plugin sandboxing and error containment

## 📄 License

This implementation is provided for research and educational purposes. Please refer to the original COMPOTE research paper for algorithmic details and citations.

## 🤝 Support

For questions, issues, or contributions:

1. **Issues**: Report bugs or request features via GitHub issues
2. **Documentation**: Check the examples and docs directories
3. **Community**: Join discussions about consensus protocol testing
4. **Research**: Reference the original COMPOTE paper for algorithmic background

## 🏆 Acknowledgments

This implementation is based on the COMPOTE research paper and implements all core algorithms with additional engineering optimizations for production use. Special thanks to the research community for advancing consensus protocol security.

---

**COMPOTE** - Making consensus protocols more robust through intelligent fuzzing. 🎯🔍🚀