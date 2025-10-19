# 📖 User Guide

Welcome to SEQ MCP Server! This guide will help you get the most out of your SEQ log analysis experience.

## 🎯 What is SEQ MCP Server?

SEQ MCP Server is a professional tool that connects your AI assistant to your SEQ log server, enabling powerful log analysis, querying, and filtering through natural language conversations.

## 🚀 Getting Started

### 1. Basic Setup

After [quick start setup](quick-start.md), you can immediately start asking your AI assistant questions about your logs:

```
"Show me all errors from the last 24 hours"
```

### 2. Understanding the Tools

SEQ MCP Server provides several specialized tools for different types of log analysis:

| Tool | Purpose | Example Use |
|------|---------|-------------|
| `seq.search` | Find specific events | "Show me all login failures" |
| `seq.tabulate` | Aggregate analysis | "Count errors by application" |
| `seq.analyze` | Complete analysis | "Analyze performance issues" |
| `seq.health` | Check connection | "Is my SEQ server healthy?" |

## 🔍 Core Features

### Search and Filter

The most common use case is searching for specific events:

```
"Find all errors containing 'timeout' in the last week"
"Show me logs from the 'web-api' application"
"Display authentication failures for user 'john.doe'"
```

### Aggregation and Analysis

Get insights from your logs with aggregation queries:

```
"Count errors by hour for the last 24 hours"
"Group logs by application and show the top 10"
"Show average response times by endpoint"
```

### Time-based Queries

Work with time ranges effectively:

```
"Show me logs from yesterday"
"Compare this week's errors with last week"
"Find patterns in the last 30 days"
```

## 📊 Advanced Features

### Multi-Window Analysis

Compare different time periods:

```
"Compare error rates between this week and last week"
"Show me performance trends across multiple time windows"
```

### Intelligent Analysis

Get comprehensive insights:

```
"Analyze the performance issues we had yesterday"
"Give me an overview of all errors this month"
"Investigate the spike in login failures"
```

### Smart Suggestions

The system provides intelligent suggestions:

```
"What should I investigate based on recent errors?"
"Suggest queries to analyze application performance"
```

## 🛠️ Best Practices

### 1. Start Specific, Then Broaden

Begin with specific queries and expand as needed:

```
❌ "Show me everything"
✅ "Show me errors from the API service in the last hour"
```

### 2. Use Time Ranges Effectively

Be specific about time periods:

```
❌ "Show me recent errors"
✅ "Show me errors from the last 2 hours"
```

### 3. Combine Search with Analysis

Get both raw data and insights:

```
"Find all timeout errors and analyze their patterns"
"Show me authentication failures and group them by user"
```

### 4. Use Filters Wisely

Narrow down results with specific filters:

```
"Show me errors from production environment only"
"Find logs with severity 'Error' or 'Critical'"
```

## 📈 Common Use Cases

### Application Monitoring

```
"Monitor my web application for errors"
"Show me performance metrics for the API"
"Alert me to unusual activity patterns"
```

### Security Analysis

```
"Find all failed authentication attempts"
"Show me suspicious login patterns"
"Analyze access patterns for admin users"
```

### Performance Analysis

```
"Identify slow database queries"
"Show me response time trends"
"Find performance bottlenecks"
```

### Troubleshooting

```
"Investigate the outage from yesterday"
"Find the root cause of the memory leak"
"Analyze the spike in error rates"
```

## 🔧 Configuration Tips

### Optimize Performance

- Use specific time ranges to limit data
- Apply filters to reduce result sets
- Use aggregation for large datasets

### Security Best Practices

- Use API keys for authentication
- Limit access to necessary log data
- Monitor your usage patterns

### Error Handling

The system provides helpful error messages:

- **Connection issues**: Check your SEQ server URL and network
- **Authentication errors**: Verify your API key
- **Query errors**: Review your search parameters

## 🛠️ Tool Reference

SEQ MCP Server provides 10 specialized tools for comprehensive log analysis:

| Tool | Purpose | Best For |
|------|---------|----------|
| `seq.health` | Verify SEQ connection and status | Initial setup verification |
| `seq.tabulate` | Analytical queries with grouping | Long-term trends, aggregations |
| `seq.search` | Targeted event search | Detailed investigation (≤24h) |
| `seq.analyze` | End-to-end analysis | Complete workflow automation |
| `seq.getEvent` | Single event details | Drill-down into specific events |
| `seq.listSignals` | Available saved signals | Using pre-configured filters |
| `seq.queryBuilder` | Optimized query construction | Guided query building |
| `seq.multiWindow` | Multi-time window analysis | Monthly/weekly comparisons |
| `seq.templates` | Query templates and examples | Learning and reference |
| `seq.currentTime` | Current date/time | Temporal analysis setup |

### Detailed Tool Descriptions

#### `seq.health`
- **Purpose**: Verifies connection and health status of SEQ
- **Use when**: Setting up connection, troubleshooting connectivity
- **Parameters**: None required
- **Example**: "Check if my SEQ server is accessible"

#### `seq.tabulate`
- **Purpose**: Analytical queries with grouping, ordering, and temporal bucketing
- **Use when**: Long time ranges (days, weeks, months), overview analysis
- **Key features**: 
  - No time limit - efficient for any range
  - Grouping by Application, Level, SourceContext, etc.
  - Temporal analysis with time granularity
  - Pagination support
- **Example**: "Show me error distribution by application for the last month"

#### `seq.search`
- **Purpose**: Targeted event search for detailed investigation
- **Use when**: Drilling down after overview analysis, specific investigations
- **Limitations**: Maximum 24h range (use seq.tabulate for longer ranges)
- **Key features**:
  - Page-based pagination
  - Bucket sampling
  - Detailed event retrieval
- **Example**: "Find all timeout errors from the API service in the last 2 hours"

#### `seq.analyze`
- **Purpose**: End-to-end analysis workflow automation
- **Use when**: Comprehensive analysis, automated workflows
- **Types**: 'errors', 'performance', 'overview', 'timeline'
- **Depths**: 'quick' (30d), 'detailed' (24h-3d), 'comprehensive' (3d)
- **Example**: "Run a comprehensive performance analysis for the last week"

#### `seq.getEvent`
- **Purpose**: Retrieve detailed information about a specific event
- **Use when**: Need full event details after getting event IDs from search
- **Features**: Always redacted (PII/secret masking, stack truncation)
- **Example**: "Get details for event evt-abc123"

#### `seq.listSignals`
- **Purpose**: List available saved signals on SEQ
- **Use when**: Want to use pre-configured filters or signals
- **Features**: Optional filtering and limiting
- **Example**: "Show me all signals related to authentication"

#### `seq.queryBuilder`
- **Purpose**: Build optimized queries with guidance
- **Use when**: Need help constructing effective queries
- **Types**: 'overview', 'drill-down', 'timeline', 'errors', 'performance'
- **Example**: "Help me build a query to analyze database performance"

#### `seq.multiWindow`
- **Purpose**: Analyze data across multiple time windows in parallel
- **Use when**: Monthly/weekly trend analysis, long-term monitoring
- **Features**: Extended window sizes (up to 1 week), intelligent caching
- **Example**: "Compare error rates across the last 4 weeks"

#### `seq.templates`
- **Purpose**: Access query templates and examples
- **Use when**: Learning, reference, or need example queries
- **Features**: Returns JSON with limits, examples, and anti-patterns
- **Example**: "Show me templates for error analysis"

#### `seq.currentTime`
- **Purpose**: Get current date/time for temporal analysis
- **Use when**: Setting up time-based queries
- **Example**: "What's the current time for setting up my analysis?"

## 📚 Query Examples

### Basic Search Examples

```
# Error Analysis
"Show me all errors from the last 24 hours"
"Find timeout errors in the API service"
"Display authentication failures for user 'john.doe'"

# Application Monitoring
"Show me logs from the 'web-api' application"
"Count errors by application for the last week"
"Monitor performance metrics for the payment service"
```

### Advanced Analysis Examples

```
# Trend Analysis
"Analyze error trends over the last month"
"Compare this week's performance with last week"
"Show daily error counts for the last 30 days"

# Aggregation Queries
"Group errors by source and show top 10"
"Count events by hour for the last 24 hours"
"Show average response times by endpoint"

# Multi-Window Analysis
"Compare error rates between this week and last week"
"Analyze performance across multiple time windows"
"Show monthly trends for database queries"
```

### Troubleshooting Examples

```
# Investigation Workflow
"Run a comprehensive analysis of yesterday's outage"
"Find the root cause of the memory leak"
"Investigate the spike in login failures"

# Security Analysis
"Find all failed authentication attempts"
"Show suspicious login patterns"
"Analyze access patterns for admin users"
```

### Performance Analysis Examples

```
# Database Performance
"Identify slow database queries"
"Show response time trends for database operations"
"Find performance bottlenecks in the data layer"

# API Performance
"Show me response times by API endpoint"
"Analyze latency patterns for external services"
"Find the slowest operations in the last hour"
```

## 📚 Learning Resources

### Troubleshooting

If you encounter issues, our [Troubleshooting Guide](troubleshooting.md) has solutions for common problems.

## 🛠️ CLI Commands

SEQ MCP Server includes a command-line interface for license management and system operations:

### Basic Commands

```bash
# Get machine ID for licensing
seq-mcp machine-id

# Check license status and expiration
seq-mcp license-status

# Show help information
seq-mcp help

# Show version information
seq-mcp version
```

### License Activation

```bash
# Activate license with JWT token
seq-mcp activate "eyJhbGc...JWT_TOKEN"
```


## 🆘 Getting Help

### Self-Service

1. Check the [Troubleshooting Guide](troubleshooting.md)
2. Review [Query Examples](#-query-examples)
3. Browse our [GitHub Issues](https://github.com/quartsystem/seq-mcp-server/issues)

### Support

- **Email**: [support@quartsystem.com](mailto:support@quartsystem.com)
- **GitHub**: [Report Issues](https://github.com/quartsystem/seq-mcp-server/issues)
- **Documentation**: This guide and related resources
- **Licensing**: All licenses currently provided free of charge

## 📄 License Terms

For complete license terms and conditions, please refer to:
- **[LICENSE](LICENSE)** - Complete license agreement (English & Italian versions)

---

**💡 Pro Tip**: The more specific you are in your queries, the better results you'll get. Start with simple questions and gradually build more complex analyses as you become familiar with the system.
