# 🚀 Getting Started with SEQ MCP Server

Quick start guide to get up and running with the SEQ MCP Server.

> 📖 **[Feature Overview](README.md)** - Complete feature documentation and tool descriptions

---

## ⚡ Quick Start (3 Minutes)

```bash
# 1. Install
dotnet tool install -g QuartSystem.SeqMcpServer

# 2. Configure via MCP client config (see below)

# 3. Use in your MCP client (Cursor, Claude Desktop, etc.)
```

**That's it!** Your 30-day free trial starts automatically. 🎉

---

## 📋 Prerequisites

- .NET 9.0 SDK or later
- Access to a SEQ server instance
- SEQ API key (optional but recommended)
- MCP-compatible client (Cursor, Claude Desktop, etc.)

---

## 📦 Installation

### Install the Global Tool

**From NuGet (Recommended):**
```bash
dotnet tool install -g QuartSystem.SeqMcpServer
```

**From Local Package:**
```bash
# Install from local .nupkg file
dotnet tool install -g --add-source ./dist QuartSystem.SeqMcpServer

# Install specific version
dotnet tool install -g --add-source ./dist QuartSystem.SeqMcpServer --version 1.1.0
```

**Verify installation PowerShell:**
```shell
dotnet tool list -g | Select-String QuartSystem.SeqMcpServer
```

**Verify installation Linux/macOS:**
```shell
dotnet tool list -g | grep QuartSystem.SeqMcpServer
```


## 🔧 Configuration

The tool needs to know your SEQ server URL and API key. You have **two options** for configuration:

### Option 1: Environment Variables (System-wide)

Set environment variables at the system level. These will be used by default:

**Linux/macOS:**
```bash
export Seq__BaseUrl="https://your-seq-server:5341"
export Seq__ApiKey="your-api-key-here"
```

**Windows PowerShell:**
```powershell
$env:Seq__BaseUrl="https://your-seq-server:5341"
$env:Seq__ApiKey="your-api-key-here"
```

**Windows CMD:**
```cmd
set Seq__BaseUrl=https://your-seq-server:5341
set Seq__ApiKey=your-api-key-here
```

### Option 2: MCP Client Configuration (Per-client)

Configure variables directly in your MCP client configuration file. **These override system environment variables.**

#### For Cursor

Edit the MCP configuration file:
- **Windows:** `%APPDATA%\.cursor\mcp.json`
- **Linux/macOS:** `~/.cursor/mcp.json`

Add this configuration:

```json
{
  "mcpServers": {
    "seq-net": {
      "type": "stdio",
      "command": "seq-mcp",
      "env": {
        "Seq__BaseUrl": "https://your-seq-server:5341",
        "Seq__ApiKey": "your-api-key-here"
      }
    }
  }
}
```

#### For Claude Desktop

Edit the MCP configuration file:
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`

Same configuration as above.

**After editing:** Restart your MCP client (Cursor, Claude Desktop, etc.)

### Configuration Priority

1. **MCP client configuration** (`env` in JSON) - **Highest priority**
2. **System environment variables** - Used if MCP config doesn't specify them
3. **Default values** - Used if neither is configured

> 💡 **Tip:** Use **Option 1** for consistent configuration across all MCP clients, or **Option 2** for client-specific settings.

### CLI Commands

Seq-MCP Server includes a command-line interface for license management:

```bash
# Activate a license key (after purchase)
seq-mcp activate <your-license-key>

# Show help information
seq-mcp help

# Show version information
seq-mcp version

# Get machine ID for licensing
seq-mcp machine-id

# Check license status and expiration
seq-mcp license-status
```

**Note**: Running `seq-mcp` without any commands starts the MCP server normally.

## ✅ Verification

### Test the Connection

Open your MCP client (e.g., Cursor) and try the following:

1. **Health Check:**
   ```
   Can you check if the SEQ server is healthy?
   ```

2. **List Signals:**
   ```
   List all available SEQ signals
   ```

3. **Simple Search:**
   ```
   Search for error logs in the last hour
   ```

> 📖 **See [README.md](README.md) for complete tool documentation and example queries.**

### Manual Testing

You can also test the tool manually from the command line:

```bash
# Set environment variables first (if not already set system-wide)
export Seq__BaseUrl="https://your-seq-server:5341"
export Seq__ApiKey="your-api-key-here"

# Run the tool (it expects JSON-RPC input on stdin)
seq-mcp
```

> 💡 **Note:** If you've configured variables in your MCP client config, those will override these environment variables when using the MCP client.

Then send a JSON-RPC request:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "seq.health",
    "arguments": {}
  }
}
```

## 📜 Licensing

### Free Trial

- ✅ **30 days free** from first use
- ✅ All features unlocked
- ✅ No credit card required
- ⏰ Trial starts automatically when you first use a tool

### After Trial

After 30 days, request a **free license** by emailing [support@quartsystem.com](mailto:support@quartsystem.com) with your Machine ID:



> 📖 **See [licensing_trial.md](licensing_trial.md) for detailed licensing information.**

---

## ❓ Common Issues

### Issue: Tool not found after installation

**Solution:** Ensure `~/.dotnet/tools` (Linux/macOS) or `%USERPROFILE%\.dotnet\tools` (Windows) is in your PATH.

**Linux/macOS:**
```bash
export PATH="$PATH:$HOME/.dotnet/tools"
```

**Windows:**
Add `%USERPROFILE%\.dotnet\tools` to your system PATH environment variable.

### Issue: "Invalid URI: The URI is empty"

**Solution:** The SEQ BaseUrl is not configured. Check that you've set it using:
- System environment variables (Option 1), or
- MCP client configuration (Option 2)

**Debugging:** If you have both configured, remember that MCP client config overrides system environment variables.

### Issue: "Unauthorized" or connection errors

**Solution:** 
- Verify your SEQ server URL is correct and accessible
- Check that your API key is valid
- If using HTTPS with self-signed certificates, you may need to set `Security:AllowInsecureDev` to `true` in development

### Issue: Rate limit errors

**Solution:** The server has built-in rate limiting to prevent abuse. If you encounter rate limit errors:

- **Wait a moment** and try again - limits reset automatically
- **Reduce the frequency** of your requests
- **Contact support** if you need higher limits for legitimate use cases

> 📖 **For advanced configuration options, see the [user-guide.md](user-guide.md)**

## 🚀 Next Steps

- Read the [README.md](README.md) for detailed feature documentation and example queries
- Check [SECURITY.md](SECURITY.md) for security best practices
- Review [licensing.md](licensing.md) for licensing details

## 💬 Support

- **Bug Reports:** [Open Issue](https://github.com/quartsystem/seq-mcp-server/issues/new?template=bug_report.md)
- **Feature Requests:** [Request Feature](https://github.com/quartsystem/seq-mcp-server/issues/new?template=feature_request.md)
- **License Issues:** [License Request](https://github.com/quartsystem/seq-mcp-server/issues/new?template=license_issue.md)
- **Documentation:** README.md, licensing_trial.md, security.md

## 📄 License Terms

For complete license terms and conditions, please refer to:
- **[LICENSE](LICENSE)** - Complete license agreement (English & Italian versions)

