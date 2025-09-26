# TMCP Transport Hook

This Python project implements the TMCP transport hook, allowing you to use MCP over TSP. To use this, you need to use [our fork](https://github.com/openwallet-foundation-labs/mcp-transport-hooks) of the MCP Python SDK that adds transport hooks to make it possible to implement TMCP. These can be installed with:

```
uv add git+https://github.com/openwallet-foundation-labs/mcp-transport-hooks git+https://github.com/openwallet-foundation-labs/tmcp-python
```

See the [`./demo` directory](https://github.com/openwallet-foundation-labs/mcp-over-tsp-python/tree/main/tmcp/demo) for some example TMCP servers and clients.

## Example usage

Below is an example TMCP server using FastMCP. By default, it will generate a DID WebVH on initial start up and host it on the [TSP test bed](https://did.teaspoon.world/). The DID will be named according to the optionally provided format string, and will contain a combination of the provided alias (set to "tmcp" by default) and a random UUIDv4 to ensure uniqueness.

```py
from mcp.server.fastmcp import FastMCP
from tmcp import TmcpManager

# Create a MCP server
mcp = FastMCP("Demo", port=8001, transport_manager=TmcpManager(alias="demo", transport="http://localhost:8001/mcp"))


# Add an addition tool
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b


if __name__ == "__main__":
    # Initialize and run the server
    mcp.run(transport="streamable-http")
```

The DID format string, DID servers, and other TMCP settings can be passed on to the `TmcpManager` during initialization. See the [`TmcpSettings`](https://github.com/openwallet-foundation-labs/tmcp-python/blob/main/src/tmcp/tmcp.py#L17) for a full list of the available settings.
