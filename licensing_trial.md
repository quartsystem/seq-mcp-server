# 🔐 Licensing Guide - SEQ MCP Server

Complete guide for licensing, trial management, and license activation.

---

## 📋 Table of Contents

1. [Trial Period](#trial-period)
2. [Requesting a License](#requesting-a-license)
3. [Installing a License](#installing-a-license)
4. [License Management](#license-management)
5. [Troubleshooting](#troubleshooting)
6. [Support](#support)

---

## 🎉 Trial Period

### 30-Day Free Trial

When you first use SEQ MCP Server, you automatically get a **30-day free trial**:

- ✅ **All features unlocked**
- ✅ **No credit card required**
- ✅ **Trial starts on first tool usage**
- ✅ **Protected against tampering** with HMAC signature
- ⏰ **Trial location**: `%LOCALAPPDATA%\.seq-mcp\license.json` (Windows) or `~/.seq-mcp/license.json` (Linux/Mac)

**Security Note:** The trial file is protected with HMAC signature to prevent modification of the trial start date. Any attempt to tamper with the file will result in the trial being considered expired.

### Trial Activation

The trial starts automatically when you first use SEQ MCP Server:

1. Install the tool
2. Configure your SEQ server connection
3. Make your first query
4. Trial is activated immediately!

### After Trial Expires

When your trial ends, you'll see this message when trying to use any tool:

```
❌ Trial period expired. Your 30-day trial has ended.

Machine ID: ABC123...XYZ==

🛒 Request a free license to continue using SEQ MCP Server:
Email: support@quartsystem.com

Thank you for trying SEQ MCP Server!
```

---

## 📧 Requesting a License

### Free License Program

**Currently, all licenses are provided FREE of charge** while QuartSystem establishes its fiscal position. This includes:

- ✅ **Personal licenses** - FREE
- ✅ **Commercial licenses** - FREE  
- ✅ **Enterprise licenses** - FREE
- ✅ **License renewals** - FREE

### How to Request a License

1. **Get Your Machine ID**
   
   Run this command to get your machine ID:
   ```bash
   seq-mcp machine-id
   ```
   
   This will output something like:
   ```
   Machine ID:
   ABC123...XYZ==
   
   Strength: Strong
   ```

2. **Send License Request Email**
   
   Send an email to: [support@quartsystem.com](mailto:support@quartsystem.com)
   
   **Subject**: "SEQ MCP Server License Request"
   
   **Include in your email**:
   - Your **Machine ID** (from step 1)
   - Your **name and email address**
   - **License type needed** (Personal/Commercial/Enterprise)
   - **Intended use** (brief description of how you'll use the tool)
   
   Example email:
   ```
   Subject: SEQ MCP Server License Request
   
   Hello,
   
   I would like to request a free license for SEQ MCP Server.
   
   Machine ID: ABC123...XYZ==
   Name: John Doe
   Email: john.doe@example.com
   License Type: Commercial
   Use Case: Log analysis for our development team
   
   Thank you!
   ```

3. **Receive Your License**
   
   You'll receive your license key via email within 24-48 hours.

4. **Activate License** (see [Installing a License](#installing-a-license))

### 🆕 Automatic License Activation

When you request a license, you may receive an activation script along with your license key. This script automatically activates your license for you.

**If you receive an activation script:**

1. **Save the script file** (e.g., `LicenseActivation365.bat` or `LicenseActivation365.sh`)
2. **Execute the script** to automatically activate your license:
   - **Windows:** Double-click the `.bat` file or run it from command line
   - **Linux/macOS:** Run `chmod +x LicenseActivation365.sh && ./LicenseActivation365.sh`
3. **Verify activation** with `seq-mcp license-status`

**If you only receive a license key:** Use the manual activation methods described below.

### License Renewals

When your license expires, simply send a renewal request to the same email address with:
- Your Machine ID
- Your previous license information
- Confirmation that you still need the license

**All renewals are currently FREE** until QuartSystem establishes its fiscal position.

---

## 📥 Installing a License

Once you receive your license key via email, install it using the `activate` command:

```bash
seq-mcp activate "YOUR_LICENSE_KEY_FROM_EMAIL"
```

### Installation Steps

1. **Run the activate command** with your license key
2. **Verify installation** with `seq-mcp license-status`
3. **Restart your MCP client** (Cursor, Claude Desktop, etc.)

---

## 🔍 License Management

### Check Your License Status

Use the CLI command to check your current license status:

```bash
seq-mcp license-status
```

This will display detailed information about your license:

**For Trial Licenses:**
```
License Status:
===============

Status: 🟡 Trial
Type: trial
Trial Started: 2025-01-15 10:30:00 UTC
Trial Ends: 2025-02-14 10:30:00 UTC
Days Remaining: 28
Machine ID: ABC123...XYZ==
```

**For Licensed Versions:**
```
License Status:
===============

Status: ✅ Licensed
Type: commercial
Expires: 2026-01-15 10:30:00 UTC
Days Remaining: 365
Machine ID: ABC123...XYZ==
```

**For Expired Licenses:**
```
License Status:
===============

Status: ❌ No Valid License
Type: None
Error: License has expired

🛒 To request a free license:
1. Get your machine ID: seq-mcp machine-id
2. Email: support@quartsystem.com
```

### Understanding License Status

- **🟡 Trial**: You're using the 30-day trial period
- **✅ Licensed**: You have a valid commercial license
- **❌ No Valid License**: Trial expired or license invalid

### License File Location

- **Windows**: `%LOCALAPPDATA%\.seq-mcp\license.json`
  - Example: `C:\Users\YourName\AppData\Local\.seq-mcp\license.json`
- **Linux**: `~/.seq-mcp/license.json`
- **macOS**: `~/.seq-mcp/license.json`

---

## 🔍 Troubleshooting

### "License file not found"

**Cause:** The file path provided doesn't exist

**Solution:**
- Check the file path is correct
- Use absolute paths if relative paths don't work
- Ensure the file exists: `ls -la license.json` (Linux/Mac) or `dir license.json` (Windows)

### "License file is empty or invalid"

**Cause:** The file doesn't contain valid license data

**Solution:**
- Ensure the file contains either:
  - A valid JWT token (starts with `eyJ`)
  - A JSON object with `licenseKey` property
- Check file permissions allow reading
- Verify the file isn't corrupted

### "Error reading license file"

**Cause:** File system or permission issues

**Solution:**
- Check file permissions: `chmod 644 license.json` (Linux/Mac)
- Ensure the file isn't locked by another process
- Try copying the file to a different location

### "License file has been tampered with"

**Cause:** The trial file has been modified or corrupted

**Solution:**
- Delete the license file and restart the application to start a new trial
- **Windows:** Delete `%LOCALAPPDATA%\.seq-mcp\license.json`
- **Linux/Mac:** Delete `~/.seq-mcp/license.json`

### "Invalid license: Signature validation failed"

**Cause:** Public/private key mismatch

**Solution:** 
- Ensure the public key in `LicenseKeys.cs` matches the private key used to generate the JWT
- Rebuild and reinstall the tool after updating the public key

### "License is bound to a different machine"

**Cause:** Machine ID doesn't match

**Solution:**
- Get the correct Machine ID from your system using `seq-mcp machine-id`
- Request a new license with the correct Machine ID
- Check license status with `seq-mcp license-status` to verify the fix

### "License has expired"

**Cause:** JWT expiration date passed

**Solution:**
- Request a license renewal by emailing [support@quartsystem.com](mailto:support@quartsystem.com)
- Include your Machine ID and previous license information
- Check license status with `seq-mcp license-status` to confirm expiration

### Trial starts even with existing license

**Cause:** License file format incorrect

**Solution:**
- Check `license.json` format matches exactly
- Ensure `licenseKey` field is present and contains valid JWT

---

## ❓ Frequently Asked Questions

### Q: What happens when my trial expires?

A: After 30 days, you'll need to request a free license to continue using SEQ MCP Server. Simply email [support@quartsystem.com](mailto:support@quartsystem.com) with your Machine ID.

### Q: Are licenses really free?

A: Yes! Currently, all licenses are provided free of charge while QuartSystem establishes its fiscal position. This includes renewals.

### Q: Do I need a license for each machine?

A: Yes, licenses are tied to specific machines. Each machine needs its own license, but all are currently free.

### Q: Can I transfer my license?

A: License transfers are possible. Contact us to discuss your situation.

### Q: What if I change machines?

A: You'll need to request a new license for the new machine. Email us with the new Machine ID.

### Q: When will licenses become paid?

A: Licenses will remain free until QuartSystem establishes a proper fiscal position. We'll provide advance notice before any changes.

### Q: How long does it take to receive a license?

A: Typically within 24-48 hours of your email request.

---

## 🆘 Support

### Trial Users

- 📖 **Documentation**: Full access to all documentation
- 🐛 **Bug Reports**: Report issues via GitHub
- 💬 **Community**: Join our community discussions

### Licensed Users

- 📖 **Documentation**: Full access to all documentation
- 🐛 **Bug Reports**: Priority handling for bug reports
- 📧 **Email Support**: Direct email support

## 📞 Contact Information

### License Requests

- **Email**: [support@quartsystem.com](mailto:support@quartsystem.com)
- **Subject**: "SEQ MCP Server License Request"

### Technical Support

- **Email**: [support@quartsystem.com](mailto:support@quartsystem.com)
- **GitHub**: [Report Issues](https://github.com/quartsystem/seq-mcp-server/issues)

### General Information

- **Website**: [quartsystem.com](https://quartsystem.com)
- **Documentation**: [GitHub Documentation](https://github.com/quartsystem/seq-mcp-server)

---

## 📄 License Terms

For complete license terms and conditions, please refer to:
- **[LICENSE](LICENSE)** - Complete license agreement (English & Italian versions)

---

**💡 Need Help?** If you have any questions about licensing, don't hesitate to contact us. We're here to help you get started with SEQ MCP Server!
