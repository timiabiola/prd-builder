# MCP Configuration Guide

Comprehensive guide for configuring and using MCP (Model Context Protocol) tools in PRD Builder projects.

---

## Architecture Overview

MCP tools are accessed through two different methods:

```
┌─────────────────────────────────────────────────────────────────┐
│                        Claude Code                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────┐  ┌─────────────────────┐  │
│  │     Docker Gateway MCP          │  │    Local MCP        │  │
│  │   (mcp__MCP_DOCKER__*)          │  │  (mcp__shadcn__*)   │  │
│  │                                  │  │                     │  │
│  │  • Playwright                    │  │  • shadcn/ui        │  │
│  │  • Context7                      │  │                     │  │
│  │  • Resend                        │  │                     │  │
│  │  • GitHub                        │  │                     │  │
│  │  • Stripe                        │  │                     │  │
│  └─────────────────────────────────┘  └─────────────────────┘  │
│              │                                                   │
│              ▼                                                   │
│  ┌─────────────────────────────────┐                            │
│  │     Docker Container             │                            │
│  │                                  │                            │
│  │  Network: host.docker.internal   │                            │
│  │  (NOT localhost!)                │                            │
│  └─────────────────────────────────┘                            │
└─────────────────────────────────────────────────────────────────┘
```

---

## Docker Gateway MCP Tools

### Prefix: `mcp__MCP_DOCKER__*`

All tools running through the Docker gateway use this prefix.

---

## Playwright MCP

Browser automation and testing tool for E2E, integration, and component testing.

### CRITICAL: Network Address Configuration

**Docker MCP runs in a container - localhost won't work!**

The Playwright MCP runs inside a Docker container, which cannot access `localhost` on your host machine. You MUST use your machine's network IP address.

| Scenario | Address | Works? |
|----------|---------|--------|
| Network IP | `192.168.x.x:3000` | YES |
| External URL | `https://example.com` | YES |
| localhost | `localhost:3000` | NO |
| loopback | `127.0.0.1:3000` | NO |

**Get Your Network Address:**
```bash
# macOS
ipconfig getifaddr en0

# Linux
hostname -I | awk '{print $1}'

# Windows
ipconfig | findstr IPv4
```

**Start Dev Server on Network:**
```bash
pnpm dev --host
# or
npm run dev -- --host
```

**Why?** Docker containers have their own network namespace. `localhost` inside Docker refers to the container itself, not your host machine. Using your network IP allows the container to reach your host's dev server.

### Common Tools

| Tool | Purpose |
|------|---------|
| `browser_navigate` | Navigate to a URL |
| `browser_snapshot` | Get accessibility snapshot (better than screenshot for actions) |
| `browser_take_screenshot` | Capture visual screenshot |
| `browser_click` | Click an element |
| `browser_type` | Type text into an element |
| `browser_fill_form` | Fill multiple form fields |
| `browser_evaluate` | Run JavaScript on the page |
| `browser_console_messages` | Get console logs |
| `browser_network_requests` | Get network requests |
| `browser_close` | Close the browser session |

### Example: Testing a Login Flow

```typescript
// 1. Navigate to local app using your NETWORK IP (e.g., 192.168.1.100)
// Get IP with: ipconfig getifaddr en0 (macOS) or hostname -I (Linux)
// Start server with: pnpm dev --host
mcp__MCP_DOCKER__browser_navigate({
  url: 'http://192.168.1.100:3000/login'  // Replace with your actual network IP
})

// 2. Get accessibility snapshot to find elements
mcp__MCP_DOCKER__browser_snapshot({})

// 3. Fill login form
mcp__MCP_DOCKER__browser_fill_form({
  fields: [
    { name: 'Email field', type: 'textbox', ref: 'input[0]', value: 'test@example.com' },
    { name: 'Password field', type: 'textbox', ref: 'input[1]', value: 'password123' }
  ]
})

// 4. Click login button
mcp__MCP_DOCKER__browser_click({
  element: 'Login button',
  ref: 'button[0]'
})

// 5. Wait for navigation
mcp__MCP_DOCKER__browser_wait_for({
  text: 'Dashboard'
})

// 6. Take screenshot for verification
mcp__MCP_DOCKER__browser_take_screenshot({
  filename: 'dashboard-after-login.png'
})

// 7. Close browser
mcp__MCP_DOCKER__browser_close({})
```

### Best Practices

1. **Always use `browser_snapshot`** before clicking/typing to get current element refs
2. **Use your network IP address** for ALL local testing (NOT localhost)
3. **Start dev server with `--host` flag** to expose to network
4. **Check console messages** for errors during testing
5. **Close browser sessions** when done to free resources

---

## Context7 MCP

Documentation and implementation guidance tool.

### Common Tools

| Tool | Purpose |
|------|---------|
| `resolve-library-id` | Find Context7-compatible library ID |
| `get-library-docs` | Get documentation for a library |

### Workflow

Always resolve the library ID first:

```typescript
// Step 1: Resolve library ID
mcp__MCP_DOCKER__resolve-library-id({
  libraryName: 'next.js'
})
// Returns: '/vercel/next.js'

// Step 2: Get documentation
mcp__MCP_DOCKER__get-library-docs({
  context7CompatibleLibraryID: '/vercel/next.js',
  topic: 'app router',
  tokens: 10000
})
```

### Common Library IDs

| Library | Context7 ID |
|---------|-------------|
| Next.js | `/vercel/next.js` |
| React | `/facebook/react` |
| Tailwind CSS | `/tailwindlabs/tailwindcss` |
| Supabase | `/supabase/supabase` |
| Stripe | `/stripe/stripe-node` |

---

## Resend MCP

Email sending tool for transactional and marketing emails.

### Tool

| Tool | Purpose |
|------|---------|
| `send-email` | Send an email |

### Example

```typescript
mcp__MCP_DOCKER__send-email({
  to: 'user@example.com',
  subject: 'Welcome to the Platform',
  text: 'Thank you for signing up!',
  html: '<h1>Welcome!</h1><p>Thank you for signing up.</p>',
  // Optional: Schedule for later
  scheduledAt: 'tomorrow at 10am'
})
```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `to` | Yes | Recipient email |
| `subject` | Yes | Email subject line |
| `text` | Yes | Plain text version |
| `html` | No | HTML version (recommended) |
| `cc` | No | CC recipients (ask user) |
| `bcc` | No | BCC recipients (ask user) |
| `scheduledAt` | No | Natural language scheduling |

---

## Local MCP Tools

### ShadCN MCP

Prefix: `mcp__shadcn__*`

ShadCN MCP runs locally, NOT through the Docker gateway. This is the only major MCP tool that uses a different prefix.

### Common Tools

| Tool | Purpose |
|------|---------|
| `search_items_in_registries` | Search for components |
| `view_items_in_registries` | View component details and source |
| `get_item_examples_from_registries` | Get usage examples/demos |
| `get_add_command_for_items` | Get CLI install command |
| `list_items_in_registries` | List all available components |
| `get_audit_checklist` | Verify new components are correct |

### Example: Adding a Dialog Component

```typescript
// 1. Search for dialog components
mcp__shadcn__search_items_in_registries({
  registries: ['@shadcn'],
  query: 'dialog'
})

// 2. View the dialog component details
mcp__shadcn__view_items_in_registries({
  items: ['@shadcn/dialog']
})

// 3. Get usage examples
mcp__shadcn__get_item_examples_from_registries({
  registries: ['@shadcn'],
  query: 'dialog-demo'
})

// 4. Get install command
mcp__shadcn__get_add_command_for_items({
  items: ['@shadcn/dialog']
})
// Returns: npx shadcn@latest add dialog

// 5. After implementing, run audit
mcp__shadcn__get_audit_checklist({})
```

---

## Quick Reference Card

### Docker Gateway Tools (`mcp__MCP_DOCKER__*`)

```
PLAYWRIGHT (use your network IP, not localhost!):
  # Get IP: ipconfig getifaddr en0 (macOS) | hostname -I (Linux)
  # Start server: pnpm dev --host
  browser_navigate({ url: 'http://192.168.x.x:3000' })  # Your network IP
  browser_snapshot({})
  browser_click({ element: 'desc', ref: 'ref' })
  browser_type({ element: 'desc', ref: 'ref', text: 'value' })
  browser_take_screenshot({ filename: 'name.png' })
  browser_close({})

CONTEXT7:
  resolve-library-id({ libraryName: 'next.js' })
  get-library-docs({ context7CompatibleLibraryID: '/vercel/next.js', topic: 'routing' })

RESEND:
  send-email({ to: 'email', subject: 'subj', text: 'body' })
```

### Local Tools (`mcp__shadcn__*`)

```
SHADCN:
  search_items_in_registries({ registries: ['@shadcn'], query: 'button' })
  view_items_in_registries({ items: ['@shadcn/button'] })
  get_item_examples_from_registries({ registries: ['@shadcn'], query: 'button-demo' })
  get_add_command_for_items({ items: ['@shadcn/button'] })
  get_audit_checklist({})
```

---

## Claude.md Template Section

Add this to every project's `Claude.md`:

```markdown
## MCP Tools Configuration

### Docker Gateway Tools (mcp__MCP_DOCKER__*)

| Tool | Purpose | When to Use |
|------|---------|-------------|
| Playwright | Testing | E2E, integration tests (MANDATORY per sprint) |
| Context7 | Documentation | Planning, debugging, implementation (MANDATORY) |
| Resend | Email | Transactional emails |

**CRITICAL: Docker MCP runs in a container - localhost won't work!**

For Playwright testing, you MUST use your network IP address:
1. Get IP: `ipconfig getifaddr en0` (macOS) or `hostname -I` (Linux)
2. Start server: `pnpm dev --host`
3. Use: `http://192.168.x.x:3000` (NOT localhost)

### Local MCP Tools (mcp__shadcn__*)

| Tool | Purpose | When to Use |
|------|---------|-------------|
| shadcn/ui | UI Components | Building/modifying UI with ShadCN |

### Sprint Testing Requirements

- 95% pass rate required before marking tasks complete
- Update `docs/checkpoint.md`, `docs/tasks.md`, `docs/planning.md` after each sprint
- Use Context7 for current documentation during planning/debugging
```

---

## Troubleshooting

### Playwright can't connect to local app

**Symptom:** `ERR_CONNECTION_REFUSED` or timeout when navigating to `localhost:3000`

**Solution:** Docker MCP runs in a container and cannot access localhost. Use your network IP instead:

1. Get your network IP:
   ```bash
   # macOS
   ipconfig getifaddr en0

   # Linux
   hostname -I | awk '{print $1}'
   ```

2. Start dev server exposed to network:
   ```bash
   pnpm dev --host
   ```

3. Use the network IP in Playwright:
   ```typescript
   mcp__MCP_DOCKER__browser_navigate({ url: 'http://192.168.1.100:3000' })
   ```

### Context7 returns no results

**Symptom:** Empty documentation returned

**Solution:**
1. First call `resolve-library-id` to get the correct ID
2. Use the exact ID format returned (e.g., `/vercel/next.js`)
3. Try a more specific `topic` parameter

### ShadCN tools not found

**Symptom:** `mcp__MCP_DOCKER__shadcn_*` tools don't exist

**Solution:** ShadCN uses local MCP, not Docker gateway. Use `mcp__shadcn__*` prefix instead.
