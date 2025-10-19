# ⚙️ Configuration Guide

Complete guide to configuring SEQ MCP Server beyond the basic setup covered in the [Quick Start Guide](quick-start.md).

## 📋 Overview

SEQ MCP Server supports advanced configuration through environment variables and MCP client configuration files. All settings have sensible defaults, so most users only need to configure the basic connection settings.

## 🔧 Configuration Methods

### Method 1: Environment Variables (System-wide)

Set environment variables at the system level for consistent configuration across all MCP clients:

**Linux/macOS:**
```bash
export Seq__BaseUrl="https://your-seq-server:5341"
export Seq__ApiKey="your-api-key-here"
export Seq__TimeoutSeconds="60"
export Seq__DefaultPageSize="200"
```

**Windows PowerShell:**
```powershell
$env:Seq__BaseUrl="https://your-seq-server:5341"
$env:Seq__ApiKey="your-api-key-here"
$env:Seq__TimeoutSeconds="60"
$env:Seq__DefaultPageSize="200"
```

### Method 2: MCP Client Configuration (Per-client)

Configure settings directly in your MCP client configuration file. **These override system environment variables.**

**For Cursor:**
```json
{
  "mcpServers": {
    "seq-net": {
      "type": "stdio",
      "command": "seq-mcp",
      "env": {
        "Seq__BaseUrl": "https://your-seq-server:5341",
        "Seq__ApiKey": "your-api-key-here",
        "Seq__TimeoutSeconds": "60",
        "Seq__DefaultPageSize": "200"
      }
    }
  }
}
```

**For Claude Desktop:**
```json
{
  "mcpServers": {
    "seq-net": {
      "type": "stdio",
      "command": "seq-mcp",
      "env": {
        "Seq__BaseUrl": "https://your-seq-server:5341",
        "Seq__ApiKey": "your-api-key-here",
        "Seq__TimeoutSeconds": "60",
        "Seq__DefaultPageSize": "200"
      }
    }
  }
}
```

## 📊 Configuration Options

### 🔗 SEQ Connection Settings

| Setting | Environment Variable | Default | Description |
|---------|---------------------|---------|-------------|
| **Base URL** | `Seq__BaseUrl` | `http://localhost:5341` | URL of your SEQ server |
| **API Key** | `Seq__ApiKey` | - | SEQ API key for authentication |
| **Timeout** | `Seq__TimeoutSeconds` | `60` | Request timeout in seconds |
| **Page Size** | `Seq__DefaultPageSize` | `200` | Default number of results per page |
| **Max Window** | `Seq__MaxWindowMinutes` | `1440` | Maximum time window for queries (24 hours) |
| **Max Rows** | `Seq__MaxRows` | `10000` | Maximum rows returned by queries |
| **Max Columns** | `Seq__MaxColumns` | `16` | Maximum columns in tabulated results |

### 🔒 Security Settings

| Setting | Environment Variable | Default | Description |
|---------|---------------------|---------|-------------|
| **Max Filter Chars** | `Security__MaxFilterChars` | `800` | Maximum characters in filter expressions |
| **Max Query Chars** | `Security__MaxQueryChars` | `1000` | Maximum characters in SQL queries |
| **Max Concurrent** | `Security__MaxConcurrentOperations` | `10` | Maximum concurrent operations |
| **Rate Limit** | `Security__MaxRequestsPerMinute` | `300` | Maximum requests per minute |

#### Per-Tool Rate Limits

You can set specific rate limits for individual tools:

```json
{
  "Security__PerToolLimits": {
    "seq.search": 50,
    "seq.tabulate": 30,
    "seq.getEvent": 100,
    "seq.listSignals": 50,
    "seq.health": 50,
    "seq.templates": 50,
    "seq.analyze": 30,
    "seq.query-builder": 30,
    "seq.multi-window": 30
  }
}
```

#### Allowed SQL Functions

Control which SQL functions are allowed in queries:

```json
{
  "Security__AllowedFunctions": ["count", "sum", "avg", "time"]
}
```

### 🔐 Data Redaction Settings

| Setting | Environment Variable | Default | Description |
|---------|---------------------|---------|-------------|
| **Enable Redaction** | `Redaction__Enable` | `true` | Enable PII and sensitive data redaction |
| **Max Stack Lines** | `Redaction__MaxStackLines` | `20` | Maximum stack trace lines to show |
| **Max Value Length** | `Redaction__MaxValueLength` | `2048` | Maximum length of property values |
| **Large Payload Limit** | `Redaction__OmitLargePayloadsOverBytes` | `65536` | Hide payloads larger than this size |

### 📈 Performance Settings

| Setting | Environment Variable | Default | Description |
|---------|---------------------|---------|-------------|
| **Top Buckets Limit** | `Bucketing__TopBucketsLimit` | `50` | Maximum number of buckets in analysis |
| **Max Representative Chars** | `Bucketing__MaxRepresentativeChars` | `180` | Maximum characters in representative samples |
| **Samples Per Bucket** | `Bucketing__DefaultSamplesPerBucket` | `3` | Default samples per bucket |

## 🎯 Common Configuration Scenarios

### Scenario 1: Basic Setup (Most Users)

**When to use:** Standard production environments, single-user setups, or when starting with SEQ MCP Server.

**Context:** Uses default settings optimized for general use cases with balanced performance and LLM efficiency.

**Configuration details:**

```bash
# Essential connection settings only
export Seq__BaseUrl="https://your-seq-server:5341"
export Seq__ApiKey="your-api-key-here"

# Default values provide good LLM context balance:
# - DefaultPageSize: 200 (reasonable data volume)
# - MaxRows: 10000 (comprehensive but not overwhelming)
# - TopBucketsLimit: 50 (good analysis depth)
```

**Benefits:**
- **Simple setup:** Minimal configuration required
- **Balanced performance:** Good compromise between data completeness and LLM efficiency
- **Proven defaults:** Settings tested for general use cases
- **LLM optimized:** Default values preserve context for quality analysis

**Considerations:**
- May need adjustment based on specific use patterns
- Monitor LLM response quality with your typical data volumes
- Consider upgrading to Scenario 2 or 3 based on usage patterns

### Scenario 2: High-Volume Environment

**When to use:** Production environments with multiple users, CI/CD pipelines, automated monitoring systems, or applications that generate high-frequency log queries.

**Context:** This configuration optimizes performance for environments where SEQ MCP Server needs to handle many concurrent requests and large datasets efficiently.

**Configuration details:**

```bash
# Increase rate limits for high-volume usage
export Seq__MaxRequestsPerMinute="600"        # Double the default (300) for busy environments
export Seq__MaxConcurrentOperations="20"      # Allow more parallel operations (default: 10)
export Seq__DefaultPageSize="500"             # Larger pages reduce round-trips (default: 200)

# Additional performance optimizations
export Seq__MaxRows="20000"                   # Allow larger result sets (default: 10000)
export Seq__TimeoutSeconds="120"              # Longer timeout for complex queries (default: 60)
export Bucketing__TopBucketsLimit="100"       # More buckets for detailed analysis (default: 50)
```

**Benefits:**
- **Higher throughput:** Supports more concurrent users and automated systems
- **Better performance:** Larger page sizes reduce network overhead
- **Scalability:** Handles enterprise-level query volumes without rate limiting issues
- **Efficiency:** Optimized for environments with predictable high usage patterns

**Considerations:**
- Monitor server resources (CPU, memory, network) with increased limits
- Ensure SEQ server can handle the increased load
- May need to adjust based on actual usage patterns
- **LLM Context Management:** Larger `DefaultPageSize` (500) and `MaxRows` (20000) can saturate LLM context windows, reducing analysis quality. Consider balancing data volume with LLM efficiency

### Scenario 3: Enterprise Security

**When to use:** Organizations with strict compliance requirements (GDPR, HIPAA, SOX), environments handling sensitive data, or when security auditing is mandatory.

**Context:** This configuration prioritizes security over performance, implementing strict controls to prevent data exposure and unauthorized access while maintaining audit trails.

**Configuration details:**

```bash
# Strict query limitations to prevent injection attacks
export Security__MaxFilterChars="400"         # Reduce from default 800 to limit complex filters
export Security__MaxQueryChars="500"           # Reduce from default 1000 to prevent SQL injection
export Security__MaxRequestsPerMinute="100"    # Lower rate limit to prevent abuse (default: 300)

# Enhanced data protection
export Redaction__Enable="true"                # Ensure PII redaction is active (default: true)
export Redaction__MaxValueLength="1024"        # Limit sensitive data exposure (default: 2048)
export Redaction__MaxStackLines="10"           # Reduce stack trace details (default: 20)
export Redaction__OmitLargePayloadsOverBytes="32768"  # Hide large payloads (default: 65536)

# Additional security hardening
export Security__MaxConcurrentOperations="5"   # Limit concurrent operations (default: 10)
export Seq__MaxWindowMinutes="480"             # Limit time windows to 8 hours (default: 1440)
export Seq__MaxRows="5000"                     # Reduce result set size (default: 10000)
```

**Benefits:**
- **Compliance ready:** Meets strict regulatory requirements for data handling
- **Attack prevention:** Limits potential vectors for injection attacks
- **Data protection:** Enhanced redaction prevents accidental PII exposure
- **Audit friendly:** Lower limits make monitoring and auditing easier

**Considerations:**
- May impact performance for legitimate high-volume usage
- Requires careful monitoring to ensure legitimate operations aren't blocked
- Consider implementing additional network-level security controls
- Regular security reviews recommended to adjust limits based on threat landscape
- **LLM Context Management:** Reduced `MaxRows` (5000) and `MaxValueLength` (1024) help preserve LLM context for better analysis, but may require multiple queries for comprehensive data exploration

### Scenario 4: Development/Testing Environment

**When to use:** Local development, testing environments, or when experimenting with SEQ queries and features.

**Context:** This configuration balances functionality with resource conservation, allowing developers to test features without impacting production systems.

**Configuration details:**

```bash
# Basic connection settings
export Seq__BaseUrl="http://localhost:5341"    # Local SEQ instance
export Seq__ApiKey="dev-api-key"               # Development API key

# Conservative limits for testing
export Seq__MaxRequestsPerMinute="60"           # Lower limit for dev environment
export Seq__MaxConcurrentOperations="3"         # Fewer concurrent operations
export Seq__DefaultPageSize="50"                # Smaller pages for faster testing
export Seq__MaxRows="1000"                      # Limited result sets

# Development-friendly settings
export Seq__TimeoutSeconds="30"                 # Shorter timeout for quick feedback
export Redaction__Enable="false"                # Disable redaction for easier debugging
export Bucketing__TopBucketsLimit="20"          # Fewer buckets for simpler analysis
```

**Benefits:**
- **Resource efficient:** Minimal impact on development machine resources
- **Fast feedback:** Shorter timeouts provide quick response during development
- **Debug friendly:** Disabled redaction makes troubleshooting easier
- **Safe testing:** Lower limits prevent accidental high-volume operations

**Considerations:**
- Not suitable for production use
- May need to adjust limits based on specific testing requirements
- Consider using separate SEQ instances for different development stages
- **LLM Context Management:** Small `DefaultPageSize` (50) and `MaxRows` (1000) optimize LLM context usage for iterative development and testing

## 🧠 LLM Context Management

**Understanding the Impact:** The size of responses from SEQ MCP Server directly affects LLM performance. When responses are too large, they consume most of the available context window, leaving little room for analysis and reducing the quality of insights.

### Key Variables Affecting LLM Context

| Variable | Impact on LLM Context | Recommendation |
|----------|---------------------|----------------|
| `Seq__DefaultPageSize` | **High** - Controls initial data volume | Start with 50-200, increase only if needed |
| `Seq__MaxRows` | **High** - Limits total result size | Balance between completeness and context efficiency |
| `Bucketing__TopBucketsLimit` | **Medium** - Affects analysis detail | Lower values preserve context for better insights |
| `Redaction__MaxValueLength` | **Medium** - Controls property value size | Shorter values reduce noise in responses |
| `Bucketing__MaxRepresentativeChars` | **Low** - Sample text length | Optimize for readability vs. context usage |

### Context Optimization Strategies

**For Analysis-Heavy Workloads:**
```bash
# Optimize for LLM analysis quality
export Seq__DefaultPageSize="100"              # Smaller initial loads
export Seq__MaxRows="5000"                      # Reasonable result limits
export Bucketing__TopBucketsLimit="30"          # Focused analysis
export Bucketing__MaxRepresentativeChars="120"  # Concise samples
```

**For Data-Heavy Workloads:**
```bash
# Balance data completeness with LLM efficiency
export Seq__DefaultPageSize="200"               # Moderate page size
export Seq__MaxRows="10000"                     # Standard limits
export Bucketing__TopBucketsLimit="50"          # Standard analysis depth
export Bucketing__MaxRepresentativeChars="180"  # Standard sample length
```

### Monitoring Context Usage

**Signs of Context Saturation:**
- LLM responses become generic or incomplete
- Analysis quality decreases with larger datasets
- LLM struggles to maintain conversation context
- Responses focus on data summary rather than insights

**Optimization Techniques:**
- Use pagination for large datasets (`DefaultPageSize`)
- Implement progressive analysis (start small, drill down)
- Leverage `Bucketing__TopBucketsLimit` to focus on most relevant data
- Consider multiple focused queries instead of one large query

## 🔍 Configuration Priority

Settings are applied in the following order (highest to lowest priority):

1. **MCP client configuration** (`env` in JSON) - **Highest priority**
2. **System environment variables** - Used if MCP config doesn't specify them
3. **Default values** - Used if neither is configured

## ✅ Verification

After configuring, test your settings:

1. **Basic Connection Test:**
   ```
   Can you check if the SEQ server is healthy?
   ```

2. **Rate Limit Test:**
   ```
   Search for logs in the last hour
   ```

3. **Security Test:**
   ```
   Try a complex query with multiple filters
   ```

## 🛠️ Troubleshooting Configuration

### Issue: Settings not taking effect

**Solution:** 
- Restart your MCP client after changing configuration
- Check that environment variable names use double underscores (`__`)
- Verify MCP client configuration syntax is valid JSON

### Issue: Rate limit errors

**Solution:**
- Increase `Security__MaxRequestsPerMinute` if you have legitimate high-volume needs
- Check `Security__PerToolLimits` for specific tool limits
- Contact support if you need enterprise-level limits

### Issue: Connection timeouts

**Solution:**
- Increase `Seq__TimeoutSeconds` for slow networks
- Check `Seq__BaseUrl` is accessible
- Verify network connectivity to SEQ server

### Issue: Large result sets

**Solution:**
- Increase `Seq__MaxRows` for larger datasets
- Adjust `Seq__DefaultPageSize` for better performance
- Consider using `Seq__MaxWindowMinutes` to limit time ranges

## 🛠️ CLI Commands

SEQ MCP Server includes a command-line interface for license management:

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

### Installation Options

```bash
# Install from NuGet (recommended)
dotnet tool install -g QuartSystem.SeqMcpServer

# Install from local package
dotnet tool install -g --add-source ./dist QuartSystem.SeqMcpServer

# Install specific version
dotnet tool install -g --add-source ./dist QuartSystem.SeqMcpServer --version 1.1.0
```

## 📚 Related Documentation

- **[Quick Start Guide](quick-start.md)** - Basic setup and installation
- **[User Guide](user-guide.md)** - Complete usage guide
- **[Troubleshooting](troubleshooting.md)** - Common issues and solutions
- **[Security](security.md)** - Security best practices

## 📄 License Terms

For complete license terms and conditions, please refer to:
- **[LICENSE](LICENSE)** - Complete license agreement (English & Italian versions)

---

> 💡 **Tip:** Start with basic configuration and only adjust advanced settings if you encounter specific issues or have special requirements.
