# Make.com to n8n Conversion Guide

## Complete Step-by-Step Process

This guide will help you convert any Make.com (formerly Integromat) workflow to n8n format.

---

## Table of Contents

1. [Understanding the Differences](#understanding-the-differences)
2. [Module Mapping Reference](#module-mapping-reference)
3. [Expression Syntax Conversion](#expression-syntax-conversion)
4. [Step-by-Step Conversion Process](#step-by-step-conversion-process)
5. [Common Issues & Solutions](#common-issues--solutions)
6. [Best Practices](#best-practices)

---

## Understanding the Differences

### Make.com Structure
```json
{
  "name": "Workflow Name",
  "flow": [
    {
      "id": 1,
      "module": "gateway:CustomWebHook",
      "parameters": {},
      "mapper": {}
    }
  ]
}
```

### n8n Structure
```json
{
  "name": "Workflow Name",
  "nodes": [
    {
      "id": "unique-id",
      "name": "Node Name",
      "type": "n8n-nodes-base.webhook",
      "parameters": {}
    }
  ],
  "connections": {}
}
```

---

## Module Mapping Reference

### Triggers

| Make.com | n8n | Notes |
|----------|-----|-------|
| `gateway:CustomWebHook` | `n8n-nodes-base.webhook` | Set `responseMode: "responseNode"` for proper responses |
| `gateway:WebhookRespond` | `n8n-nodes-base.respondToWebhook` | Used to send responses back |
| `builtin:BasicScheduler` | `n8n-nodes-base.scheduleTrigger` | Cron-based scheduling |

### Actions

| Make.com | n8n | Notes |
|----------|-----|-------|
| `http:ActionSendData` | `n8n-nodes-base.httpRequest` | Remove `authentication` and `genericAuthType` params |
| `datastore:AddRecord` | `n8n-nodes-base.googleSheets` or database nodes | Use Google Sheets, PostgreSQL, MySQL, etc. |
| `json:ParseJSON` | `n8n-nodes-base.code` | Use JavaScript to parse |
| `builtin:BasicRouter` | `n8n-nodes-base.switch` | Route based on conditions |

### Flow Control

| Make.com | n8n | Notes |
|----------|-----|-------|
| Filter conditions | `n8n-nodes-base.if` | Use IF node for conditional logic |
| Iterator | `n8n-nodes-base.splitInBatches` | Process arrays |
| Aggregator | `n8n-nodes-base.aggregate` | Combine data |

---

## Expression Syntax Conversion

### Variable References

| Make.com | n8n | Description |
|----------|-----|-------------|
| `{{1.email}}` | `={{ $('Node Name').item.json.email }}` | Reference specific node output |
| `{{1.data.contact.id}}` | `={{ $('Node Name').item.json.data.contact.id }}` | Nested property access |
| `{{now}}` | `={{ $now }}` | Current timestamp |
| `{{formatDate(now; "X")}}` | `={{ Math.floor(Date.now() / 1000) }}` | Unix timestamp |

### String Concatenation

**Make.com:**
```
"Welcome, {{1.first_name}}!"
```

**n8n (in body parameters):**
```javascript
"={{ 'Welcome, ' + $('Webhook').item.json.body.first_name + '!' }}"
```

### Conditional Expressions

**Make.com:**
```json
{
  "a": "{{1.email}}",
  "b": "",
  "o": "text:notequal"
}
```

**n8n:**
```json
{
  "value1": "={{ $json.body.email }}",
  "operation": "isNotEmpty"
}
```

---

## Step-by-Step Conversion Process

### Step 1: Analyze the Make.com Blueprint

1. **Export the blueprint** from Make.com
2. **Identify all modules** in the flow
3. **Map module IDs** to understand data flow
4. **Note all variable references** ({{1.field}}, {{2.data}}, etc.)

### Step 2: Create n8n Workflow Structure

```json
{
  "name": "Workflow Name",
  "nodes": [],
  "connections": {},
  "pinData": {},
  "settings": {
    "executionOrder": "v1"
  },
  "staticData": null,
  "tags": [],
  "triggerCount": 0
}
```

### Step 3: Convert Each Module to n8n Node

#### Example: Webhook Trigger

**Make.com:**
```json
{
  "id": 1,
  "module": "gateway:CustomWebHook",
  "parameters": {
    "hook": 2140292,
    "maxResults": 1
  }
}
```

**n8n:**
```json
{
  "parameters": {
    "httpMethod": "POST",
    "path": "webhook-path",
    "responseMode": "responseNode",
    "options": {}
  },
  "id": "webhook-node",
  "name": "Webhook",
  "type": "n8n-nodes-base.webhook",
  "typeVersion": 1.1,
  "position": [240, 300],
  "webhookId": "webhook-path"
}
```

#### Example: HTTP Request

**Make.com:**
```json
{
  "id": 2,
  "module": "http:ActionSendData",
  "parameters": {},
  "mapper": {
    "url": "https://api.example.com/endpoint",
    "method": "post",
    "headers": [
      {
        "name": "Authorization",
        "value": "Bearer token"
      }
    ],
    "data": "{\"field\":\"{{1.value}}\"}"
  }
}
```

**n8n:**
```json
{
  "parameters": {
    "method": "POST",
    "url": "https://api.example.com/endpoint",
    "sendHeaders": true,
    "headerParameters": {
      "parameters": [
        {
          "name": "Authorization",
          "value": "Bearer token"
        }
      ]
    },
    "sendBody": true,
    "bodyParameters": {
      "parameters": [
        {
          "name": "field",
          "value": "={{ $('Webhook').item.json.body.value }}"
        }
      ]
    },
    "options": {
      "response": {
        "response": {
          "responseFormat": "json"
        }
      }
    }
  },
  "id": "http-request-node",
  "name": "HTTP Request",
  "type": "n8n-nodes-base.httpRequest",
  "typeVersion": 4.1,
  "position": [460, 300]
}
```

### Step 4: Convert Filters to IF Nodes

**Make.com Filter:**
```json
{
  "filter": {
    "name": "Only process if email exists",
    "conditions": [
      [
        {
          "a": "{{1.email}}",
          "b": "",
          "o": "text:notequal"
        }
      ]
    ]
  }
}
```

**n8n IF Node:**
```json
{
  "parameters": {
    "conditions": {
      "string": [
        {
          "value1": "={{ $json.body.email }}",
          "operation": "isNotEmpty"
        }
      ]
    }
  },
  "id": "if-node",
  "name": "Check Email",
  "type": "n8n-nodes-base.if",
  "typeVersion": 1,
  "position": [460, 300]
}
```

### Step 5: Create Connections

Map the flow from Make.com to n8n connections:

**Make.com Flow:**
- Module 1 → Module 2 → Module 3

**n8n Connections:**
```json
{
  "connections": {
    "Node 1": {
      "main": [
        [
          {
            "node": "Node 2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Node 2": {
      "main": [
        [
          {
            "node": "Node 3",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  }
}
```

### Step 6: Handle Data Storage

**Make.com Datastore:**
```json
{
  "module": "datastore:AddRecord",
  "parameters": {
    "datastore": 90626
  },
  "mapper": {
    "key": "{{2.contact.id}}",
    "data": {
      "email": "{{1.email}}",
      "name": "{{1.name}}"
    }
  }
}
```

**n8n Google Sheets:**
```json
{
  "parameters": {
    "operation": "append",
    "documentId": {
      "__rl": true,
      "value": "YOUR_SHEET_ID",
      "mode": "list"
    },
    "sheetName": {
      "__rl": true,
      "value": "Sheet1",
      "mode": "list"
    },
    "columns": {
      "mappingMode": "defineBelow",
      "value": {
        "contactId": "={{ $('Create Contact').item.json.contact.id }}",
        "email": "={{ $('Webhook').item.json.body.email }}",
        "name": "={{ $('Webhook').item.json.body.name }}"
      }
    }
  },
  "type": "n8n-nodes-base.googleSheets",
  "typeVersion": 4.4
}
```

---

## Common Issues & Solutions

### Issue 1: Authentication Headers Not Working

**Problem:**
```json
{
  "authentication": "genericCredentialType",
  "genericAuthType": "httpHeaderAuth"
}
```

**Solution:**
Remove these parameters and use direct header configuration:
```json
{
  "sendHeaders": true,
  "headerParameters": {
    "parameters": [
      {
        "name": "Authorization",
        "value": "Bearer token"
      }
    ]
  }
}
```

### Issue 2: Variable References Undefined

**Problem:**
```javascript
"={{ $json.data.contact.id }}"  // Returns undefined
```

**Solution:**
Check the actual data structure from the previous node:
```javascript
"={{ $json.contact.id }}"  // Correct path
```

### Issue 3: String Concatenation in Body Parameters

**Problem:**
```javascript
"Welcome, {{ $('Webhook').item.json.body.first_name }}!"  // Doesn't work
```

**Solution:**
```javascript
"={{ 'Welcome, ' + $('Webhook').item.json.body.first_name + '!' }}"
```

### Issue 4: Duplicate Contact Errors

**Problem:**
API returns 400 error for duplicate contacts

**Solution:**
Add error handling with `ignoreResponseCode: true` option or use IF node to check for duplicates first.

---

## Best Practices

### 1. Node Naming
- Use descriptive names: "Create Contact" instead of "HTTP Request 1"
- Match the action being performed
- Keep names consistent across workflows

### 2. Error Handling
- Add IF nodes for validation
- Use `respondToWebhook` for proper error responses
- Set appropriate HTTP status codes (400, 404, 500)

### 3. Data Flow
- Always check the output structure of each node
- Use the "Execute Node" feature to test
- Reference nodes by name, not by position

### 4. Expression Syntax
- Use `={{ }}` for all expressions
- Test expressions in the expression editor
- Use string concatenation with `+` operator

### 5. Credentials
- Replace hardcoded tokens with n8n credentials
- Use environment variables for sensitive data
- Never commit credentials to version control

### 6. Testing
- Test each node individually
- Use unique test data to avoid duplicates
- Check all branches (success and error paths)

---

## Conversion Checklist

- [ ] Export Make.com blueprint
- [ ] Identify all modules and their types
- [ ] Map module IDs to data flow
- [ ] Create n8n workflow structure
- [ ] Convert trigger module
- [ ] Convert action modules
- [ ] Convert filters to IF nodes
- [ ] Update all variable references
- [ ] Fix expression syntax
- [ ] Create node connections
- [ ] Handle data storage
- [ ] Add error handling
- [ ] Test with sample data
- [ ] Replace placeholder credentials
- [ ] Document any custom logic
- [ ] Test end-to-end workflow

---

## Quick Reference: Common Conversions

### Webhook Response
```json
// Make.com: Automatic
// n8n: Add respondToWebhook node
{
  "parameters": {
    "respondWith": "json",
    "responseBody": "={{ { \"success\": true, \"data\": $json } }}"
  },
  "type": "n8n-nodes-base.respondToWebhook"
}
```

### Date Formatting
```javascript
// Make.com: {{formatDate(now; "YYYY-MM-DD")}}
// n8n: ={{ $now.toFormat('yyyy-MM-dd') }}
```

### Array Iteration
```javascript
// Make.com: Iterator module
// n8n: Use SplitInBatches or loop in Code node
```

### JSON Parsing
```javascript
// Make.com: json:ParseJSON
// n8n: Use Code node with JSON.parse()
```

---

## Resources

- **n8n Documentation**: https://docs.n8n.io
- **n8n Community**: https://community.n8n.io
- **n8n Expression Reference**: https://docs.n8n.io/code-examples/expressions/
- **Make.com Documentation**: https://www.make.com/en/help

---

## Example: Complete Conversion

See the `Nexa_Lead_Capture_Follow-up_n8n.json` file for a complete example of a converted workflow including:
- Webhook trigger
- Email validation
- Multiple HTTP requests
- Google Sheets integration
- Error handling
- Webhook responses

---

## Support

For questions or issues with conversions:
1. Check the n8n documentation
2. Search the n8n community forum
3. Review this guide's troubleshooting section
4. Test each node individually to isolate issues

---

**Last Updated**: May 23, 2026
**Version**: 1.0