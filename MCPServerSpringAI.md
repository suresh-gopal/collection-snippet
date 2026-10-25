Objective

This repository contains a minimal MCP server built with Java, Spring Boot and Spring AI. The MCP server exposes order shipping and fulfillment data (order status, tracking numbers, carrier, estimated delivery, etc.) as MCP tools that AI assistants (e.g., GitHub Copilot, VS Code AI features) can call.

What is included

- A Spring Boot application (McpServerApplication) that registers tool callbacks using MethodToolCallbackProvider.
- OrderMcpServerService exposes a getOrderStatus method annotated with @Tool to let AI assistants query order fulfillment information.
- orders.json sample data under src/main/resources.
- application.yaml configures spring.ai.mcp.server name and version.
- mcp.json lists the server name (order-inventory) for MCP discovery.

How to run

1. Build with Maven: mvn -f MCPServer clean package
2. Run: java -jar MCPServer/target/mcp-server-0.1.0.jar

Running smoke tests

The MCP server includes a Spring Boot integration test that validates the getOrderStatus tool handler:

1. Navigate to the MCPServer directory: cd MCPServer
2. Run the smoke test: mvn test -Dtest=McpStdioClientSmokeTest

This test verifies:
- getOrderStatus returns correct data for order 1001 (SHIPPED)
- getOrderStatus returns correct data for order 1002 (FULFILLED)
- getOrderStatus handles non-existent orders (NOT_FOUND)
- All required fields are present in responses (orderId, status, customer, items, trackingNumber, carrier, estimatedDelivery)

Expected test output:
```
Tests run: 5, Failures: 0
✓ testGetOrderStatusFor1001 PASSED
✓ testGetOrderStatusFor1002 PASSED
✓ testGetOrderStatusForNonExistent PASSED
✓ testGetOrderStatusWithNullOrderId PASSED
✓ testGetOrderStatusReturnsAllFields PASSED
```

AI Assistant Integration

The MCP server is configured for integration with GitHub Copilot CLI and Visual Studio Code. The following mcp.json configuration files have been created:

**VSCode Configuration**
- File: `mcp.json` (in repository root)
- Enables AI assistants in VS Code to call getOrderStatus through the MCP protocol
- The server is launched via: `java -jar MCPServer/target/mcp-server-0.1.0.jar`

**GitHub Copilot CLI Configuration**
- File: `~/.copilot/mcp.json`
- Provides Copilot CLI with access to the order-inventory MCP server
- Alternative config: `~/.copilot/mcp-config.json`

Add the following configuration to `~/.copilot/mcp.json`:

```json
{
  "mcpServers": {
    "order-inventory": {
      "command": "java",
      "args": [
        "-jar",
        "\\MCPServer\\target\\mcp-server-0.1.0.jar"
      ],
      "env": {
        "JAVA_TOOL_OPTIONS": "-Xmx512m"
      },
      "description": "Exposes order and inventory fulfillment data to AI assistants"
    }
  }
}
```

Configuration Details:
- **command**: Java executable to launch the MCP server
- **args**: JAR file path (adjust to your system path)
- **env**: Environment variables (512MB heap allocation)
- **description**: Tool description for Copilot

Both configurations point to the same MCP server and enable AI assistants to query order information directly. For example:
```
copilot
> What is the status of order 1001?
```

The AI assistant will use the getOrderStatus tool to retrieve and return the order details.

Notes

Adjust dependency versions in pom.xml to match the Spring Boot and Spring AI versions you want to use. The code uses Jackson for JSON parsing and a simplified MethodToolCallbackProvider builder — adapt wiring to match the actual Spring AI MCP starter API in your environment.