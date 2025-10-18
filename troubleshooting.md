# 🛠️ Troubleshooting Guide

This guide helps you resolve common issues with SEQ MCP Server.

## 🚨 Quick Fixes

### Connection Issues

**Problem**: "Cannot connect to SEQ server"

**Solutions**:
1. **Check your SEQ server URL**
   ```bash
   # Verify the URL is correct and accessible
   curl -I https://your-seq-server.com
   ```

2. **Verify network connectivity**
   ```bash
   # Test if you can reach the server
   ping your-seq-server.com
   ```

3. **Check firewall settings**
   - Ensure port 443 (HTTPS) or 80 (HTTP) is open
   - Check if your firewall blocks outbound connections

### Authentication Issues

**Problem**: "Authentication failed" or "Invalid API key"

**Solutions**:
1. **Verify your API key**
   - Check if the key is correct in your configuration
   - Ensure the key hasn't expired
   - Test the key directly with SEQ API

2. **Check API key permissions**
   - Ensure your API key has the necessary permissions
   - Contact your SEQ administrator if needed

### Installation Issues

**Problem**: "Command not found" or installation fails

**Solutions**:
1. **Check .NET installation**
   ```bash
   dotnet --version
   # Should show .NET 9.0 or later
   ```

2. **Reinstall the tool**
   ```bash
   dotnet tool uninstall -g QuartSystem.SeqMcpServer
   dotnet tool install -g QuartSystem.SeqMcpServer
   ```

3. **Check PATH environment**
   - Ensure .NET tools directory is in your PATH
   - Restart your terminal/command prompt

## 🔍 Common Error Messages

### "License expired"

**What it means**: Your trial or license has expired.

**Solution**: Request a free license by emailing [support@quartsystem.com](mailto:support@quartsystem.com) with your Machine ID.

### "Machine ID mismatch"

**What it means**: Your license is tied to a different machine.

**Solution**: Contact support to transfer your license to the current machine.

### "Query timeout"

**What it means**: Your query took too long to execute.

**Solutions**:
1. **Reduce time range**
   - Use shorter time periods
   - Try "last hour" instead of "last week"

2. **Add filters**
   - Filter by specific applications
   - Use more specific search terms

3. **Reduce result count**
   - Limit results to smaller numbers
   - Use pagination for large datasets

### "Rate limit exceeded"

**What it means**: You've made too many requests too quickly.

**Solutions**:
1. **Wait a few minutes** before trying again
2. **Reduce query frequency**
3. **Contact support** if you need higher limits

### "Invalid query syntax"

**What it means**: Your search query has syntax errors.

**Solutions**:
1. **Simplify your query**
   - Start with basic searches
   - Add complexity gradually

2. **Check query examples**
   - Review our [Query Examples](user-guide.md#-query-examples)
   - Use the query builder tool

## 🔧 Configuration Issues

### MCP Client Not Detecting SEQ MCP Server

**Problem**: Your MCP client doesn't recognize the SEQ MCP Server.

**Solutions**:
1. **Check configuration file**
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

2. **Verify command path**
   ```bash
   which seq-mcp
   # Should show the path to the installed tool
   ```

3. **Restart MCP client**
   - Close and reopen your MCP client
   - Check client logs for error messages

### Configuration Not Loading

**Problem**: Your configuration settings aren't being applied.

**Solutions**:
1. **Check file location**
   - Ensure config file is in the correct location
   - Verify file permissions

2. **Validate JSON syntax**
   - Use a JSON validator to check syntax
   - Ensure proper escaping of special characters

3. **Check environment variables**
   - Verify environment variables are set correctly
   - Restart your terminal after setting variables

## 📊 Performance Issues

### Slow Queries

**Problem**: Queries take too long to execute.

**Solutions**:
1. **Optimize time ranges**
   - Use shorter time periods
   - Start with recent data

2. **Add specific filters**
   - Filter by application or service
   - Use exact matches when possible

3. **Use aggregation**
   - Group results instead of returning raw data
   - Use count/sum operations for large datasets

### High Memory Usage

**Problem**: SEQ MCP Server uses too much memory.

**Solutions**:
1. **Reduce result limits**
   - Set lower max-results values
   - Use pagination for large datasets

2. **Optimize queries**
   - Use more specific searches
   - Avoid very broad time ranges

## 🆘 Getting Additional Help

### Self-Service Resources

1. **Documentation**
   - [User Guide](user-guide.md)
   - [Tool Reference](user-guide.md#-tool-reference)
   - [Query Examples](user-guide.md#-query-examples)

2. **Community Support**
   - [GitHub Issues](https://github.com/quartsystem/seq-mcp-server/issues)
   - [GitHub Discussions](https://github.com/quartsystem/seq-mcp-server/discussions)

### Professional Support

**Email Support**:
- **General Issues**: [support@quartsystem.com](mailto:support@quartsystem.com)
- **Licensing**: [support@quartsystem.com](mailto:support@quartsystem.com) (All licenses currently free)

**When contacting support, please include**:
- Your SEQ server version and URL
- Your operating system and .NET version
- The exact error message you're seeing
- Steps to reproduce the issue
- Your configuration (without sensitive information)

---

## 📄 License Terms

For complete license terms and conditions, please refer to:
- **[LICENSE](LICENSE)** - Complete license agreement (English & Italian versions)

---

**💡 Pro Tip**: Most issues can be resolved by checking your configuration, verifying network connectivity, and ensuring your SEQ server is accessible. When in doubt, start with the basics and work your way up to more complex solutions.
