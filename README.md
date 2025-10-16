# MCP Rust POC

Create a simple MPC Server written in Rust to increment, decrement and echo out some message.

# Building binary file

**Run this command to create exec file in `target/release/`**

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

# Code Breakdown

### `#[tool_router]` - acts as the bridge between your Rust code and the AI's ability to discover, understand, and execute your custom tools within the MCP ecosystem. In which it wires up the MCP protocol for any methods with `#[tool]`

### `#[tool]` - it defines a tool that can be called by the AI. It's like a function that you want to make available for use in your conversation with Claude. With the `description ` attribute, you provide a brief description of what the tool does.

```rust
#[tool_router]
impl Counter {
    fn new() -> Self {
        Self {
            counter: Arc::new(Mutex::new(0)),
            tool_router: Self::tool_router(),
        }
    }

     #[tool(description = "Increment the counter by 1")]
    async fn increment(&self) -> Result<CallToolResult, McpError> {
        let mut counter = self.counter.lock().await;
        *counter += 1;
        Ok(CallToolResult::success(vec![Content::text(
            counter.to_string(),
        )]))
    }
}
```

`#[derive(Clone)]` - Clone is needed for the server state to be shared across requests

```rust

#[derive(Clone)]
pub struct Counter {
    counter: Arc<Mutex<i32>>,
    tool_router: ToolRouter<Self>,
}
```
