# Burp Suite - Part 1: Proxy, Intercept, Repeater, and sqlmap Integration

A practical guide to using Burp Suite for intercepting HTTP requests, analyzing web traffic, modifying requests with Repeater, and integrating captured requests with sqlmap for SQL injection testing.

---

# Overview

This documentation provides a step-by-step guide to configuring Burp Suite on Kali Linux, intercepting HTTP traffic, analyzing and modifying requests using Repeater, and exporting requests for SQL injection testing with sqlmap.

> **Note**
>
> This project is intended for educational purposes and should only be used on systems you own or have explicit authorization to test.

---

# Prerequisites

Before starting, ensure the following requirements are met:

- Kali Linux installed
- Burp Suite Community or Professional Edition
- Docker installed and running
- PHP & MySQL container configured
- sqlmap installed
- A local web application running on Docker

---

# Step 1: Build the Local Testing Environment

1. Start the Docker service.

2. Launch your PHP and MySQL containers.

3. Verify that your web application is accessible through your browser.

Example:

```
http://localhost:8080
```

---

# Step 2: Configure Burp Suite

1. Launch Burp Suite.

2. Create a **Temporary Project**.

3. Click **Next**.

4. Click **Start Burp**.

5. Open:

```
Proxy → Intercept
```

6. Click **Open Browser**.

Burp Browser is already configured to communicate with Burp Suite, so no manual proxy configuration is required.

---

# Step 3: Capture HTTP Requests

1. Open the Burp Browser.

2. Navigate to:

```
http://localhost:8080
```

3. Ensure the **Intercept is on** button is enabled.

4. Browse your web application.

Burp Suite will capture every HTTP request before it reaches the server.

---

# Using Intercept

The Intercept feature allows you to inspect every request before it is sent to the server.

You can:

- View HTTP headers
- View cookies
- View request parameters
- Modify submitted values
- Forward or drop requests

---

# Using Repeater

Repeater is used to resend and modify HTTP requests manually.

## Sending Requests to Repeater

1. Capture a request.

2. Right-click the request.

3. Select:

```
Send to Repeater
```

---

## Testing Requests

1. Open the **Repeater** tab.

2. Edit the request on the left panel.

3. Click **Send**.

4. Review the server response on the right panel.

### Request Panel

Contains the HTTP request sent to the server.

### Response Panel

Displays the server's response after processing the request.

---

# User-Agent Manipulation Example

Locate the following header:

```
User-Agent: Mozilla/5.0 ...
```

Replace it with:

```
User-Agent: FarisGanteng-Agent-007
```

Click **Send**.

Observe how the server processes the modified User-Agent header.

---

# Burp Suite and sqlmap Integration

## Why Combine Burp Suite with sqlmap?

Burp Suite allows manual analysis and precise request modification, while sqlmap automates SQL injection detection and exploitation.

Using both tools together provides greater flexibility and accuracy during security assessments.

| Tool | Primary Function |
|------|------------------|
| Burp Suite | Intercept, inspect, and manually modify HTTP requests |
| sqlmap | Automatically detect and test SQL injection vulnerabilities |

---

# Advantages

- Capture only the requests you want to test.
- Modify headers, cookies, and parameters before testing.
- Validate server responses manually.
- Reduce unnecessary automated scanning.

---

# Exporting Requests

1. Capture a request in Burp Suite.

2. Right-click the request.

3. Select:

```
Copy to file
```

or manually copy the full HTTP request into a file named:

```
request.txt
```

---

# Running sqlmap

Example:

```bash
sqlmap -r request.txt --batch --dbs
```

Explanation:

| Option | Description |
|---------|-------------|
| `-r` | Reads the HTTP request from a file |
| `--batch` | Runs sqlmap without interactive prompts |
| `--dbs` | Enumerates available databases |

---

# Workflow Summary

1. Start Docker.
2. Launch the PHP & MySQL application.
3. Open Burp Suite.
4. Capture an HTTP request.
5. Analyze or modify the request using Repeater.
6. Export the request.
7. Run sqlmap using the exported request.
8. Review the results.

---

# Common Issues

| Problem | Solution |
|---------|----------|
| No requests captured | Ensure **Intercept is on** and use Burp Browser. |
| Website cannot be reached | Verify Docker containers are running. |
| Burp Browser does not load | Restart Burp Suite or create a new temporary project. |
| sqlmap cannot read the request | Verify that `request.txt` contains the complete HTTP request. |

---

# Conclusion

This project demonstrates the fundamental workflow of Burp Suite for intercepting HTTP traffic, modifying requests with Repeater, and integrating captured requests with sqlmap for SQL injection testing in a local development environment.

It serves as an introductory practice for learning web application security testing using Burp Suite and sqlmap.
