# unreal-blueprint-reader

C++ Unreal Engine editor plugin that exposes Blueprint graph structure as JSON for AI tooling via Python scripting.

Gives AI assistants structural access to Blueprint graphs — nodes, pins, connections, execution flow, and variables — through `UFUNCTION`s callable from the editor's Python environment.

## Why?

Blueprint graphs are opaque binary assets. AI assistants can't read `.uasset` files, so they're blind to any logic implemented in Blueprints. This plugin serializes the graph data to JSON, making it accessible through the UE Python scripting API.

**Companion MCP server:** [unreal-blueprint-mcp](https://github.com/tumourlove/unreal-blueprint-mcp) wraps these functions as MCP tools for AI assistants.

## Installation

Copy or symlink this plugin to your project's `Plugins/` folder:

```
MyProject/
  Plugins/
    BlueprintReader/
      BlueprintReader.uplugin
      Source/
        BlueprintReader/
          ...
```

Recompile the project or restart the editor.

## Prerequisites

- **Unreal Engine 5.x** (tested on 5.7)
- **Python Remote Execution** enabled in Project Settings (for AI tooling access)

## Python-Callable Functions

All functions are static methods on `UBlueprintReaderLibrary`:

| Function | Description |
|----------|-------------|
| `get_blueprint_graph_list(asset_path)` | List all graphs (event graphs, functions, macros) in a Blueprint |
| `get_graph_data(asset_path, graph_name)` | Full node/pin/connection serialization for a graph |
| `get_blueprint_variables(asset_path)` | All variables with types, defaults, and property flags |
| `get_execution_flow(asset_path, entry_point)` | Linearized execution trace from an entry event/function |
| `search_nodes(asset_path, query)` | Search nodes by title, class, or function name |

### Example (editor Python console)

```python
import unreal
result = unreal.BlueprintReaderLibrary.get_blueprint_graph_list('/Game/Characters/BP_Hero')
print(result)
```

## License

MIT
