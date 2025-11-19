# logs-explorer

AI-powered log exploration and analysis using OpenTelemetry MCP Server. Supercharge your log searching with natural language queries and intelligent insights.

## Overview

This repository provides comprehensive guides and configurations for integrating `otel-mcp-server` (OpenTelemetry MCP Server) with your existing observability stack (Kibana, OpenObserve, Elasticsearch) to enable AI-powered log analysis.

### What You Get

- 🤖 **Natural Language Queries**: Ask questions in plain English instead of writing complex Elasticsearch DSL
- 🔍 **Intelligent Pattern Detection**: AI-powered anomaly detection and trend analysis
- ⚡ **Faster Debugging**: Get insights quickly without switching between multiple tools
- 🔗 **Seamless Integration**: Works alongside your existing Kibana and OpenObserve dashboards
- 🎯 **Context-Aware Analysis**: AI understands relationships between logs across services

## Quick Start

1. **Configure your MCP client** (Claude Desktop, Cline, etc.) with your Elasticsearch credentials
2. **Connect to your log data** (Elasticsearch, Kibana, or OpenObserve)
3. **Start querying** using natural language

See the [Setup Guide](otel-mcp-server-setup.md) for detailed instructions.

## Documentation

- **[Setup Guide](otel-mcp-server-setup.md)** - Complete installation and configuration instructions
- **[Configuration Examples](config-examples.md)** - Sample configurations for various scenarios
- **[Query Examples](query-examples.md)** - Learn how to query logs using natural language

## Example Usage

Instead of writing complex Elasticsearch queries, simply ask:

```
Show me all ERROR logs from the payment-service in the last hour
```

```
What caused the spike in errors at 2:30 PM?
```

```
Find all logs related to request ID abc-123-def
```

```
Are there any unusual patterns in today's logs?
```

## How It Works

```
┌─────────────────┐
│   AI Assistant  │ (Claude Desktop, Cline, etc.)
│  (MCP Client)   │
└────────┬────────┘
         │ Natural Language Queries
         │
┌────────▼────────┐
│ otel-mcp-server │ Translates queries to Elasticsearch DSL
└────────┬────────┘
         │
         ├──────────────┬─────────────┬─────────────┐
         │              │             │             │
    ┌────▼────┐   ┌────▼─────┐  ┌───▼──────┐ ┌────▼────────┐
    │Elasticsearch│ │  Kibana  │  │OpenObserve│ │Your Logs   │
    └────────────┘ └──────────┘  └───────────┘ └─────────────┘
```

## Use Cases

### Development
- Debug issues in real-time with conversational queries
- Understand log patterns during development
- Trace requests across microservices

### DevOps
- Investigate production incidents faster
- Identify anomalies and trends
- Monitor deployment impacts

### Team Collaboration
- Share natural language queries instead of complex DSL
- Document common debugging patterns
- Onboard team members faster

## Benefits Over Traditional Approaches

| Traditional (Kibana/OpenObserve) | AI-Powered (otel-mcp-server) |
|----------------------------------|------------------------------|
| Manual query construction | Natural language queries |
| Need to know query syntax | Conversational interface |
| Switch between tools | Integrated in development environment |
| Point-and-click exploration | AI-guided investigation |
| Manual pattern recognition | Automated insights |

## Compatibility

- ✅ Elasticsearch (any version)
- ✅ Kibana
- ✅ OpenObserve
- ✅ Elastic Cloud
- ✅ Self-hosted Elasticsearch clusters

## Requirements

- Node.js v18 or higher
- Access to Elasticsearch (via Kibana, OpenObserve, or direct)
- MCP-compatible client (Claude Desktop, Cline, etc.)

## Getting Help

- Check the [Setup Guide](otel-mcp-server-setup.md) for troubleshooting
- Review [Configuration Examples](config-examples.md) for your specific setup
- Try [Query Examples](query-examples.md) to learn effective querying

## Resources

- [OpenTelemetry MCP Server Article](https://dev.to/shiftyp/supercharge-your-observability-how-otel-mcp-server-unlocks-ai-powered-insights-5dii) - Deep dive into AI-powered observability
- [Model Context Protocol](https://modelcontextprotocol.io/) - Learn about MCP
- [Claude Desktop](https://claude.ai/desktop) - Popular MCP client
- [Elasticsearch Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

## Contributing

This project was created using GitHub Copilot. Contributions, suggestions, and improvements are welcome!

## License

MIT

---

**Ready to supercharge your log searching?** Start with the [Setup Guide](otel-mcp-server-setup.md)!