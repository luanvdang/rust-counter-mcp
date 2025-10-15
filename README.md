# MCP Rust POC
 Create a simple MPC Server written in Rust to increment, decrement and echo out some message.
 
# Building binary file

**Run this command to create exec file in target/release/**

```
cargo build --release --quiet
```

# Add this path to Claude Desktop config file

```json

"mcpServers": {
    "counter": {
      "args": [],
      "command": "/path/to/target/release/counter"
    }
}

```



