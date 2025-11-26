# Lab 06 (Part 1) – Creating your own MCP server

## 1. What Is MCP?

The Model Context Protocol (MCP) is a standard that lets AI agents (clients) discover and call capabilities exposed by external processes (servers) in a consistent way. Instead of writing bespoke integrations for each tool or data source, you expose:

Think of MCP as “USB for AI tooling”: a universal plug so clients (like GitHub Copilot, custom scripts, or other agent frameworks) can enumerate and invoke functionality.

### **🧱 High-Level MCP Architecture Overview**

MCP follows a **client-server model**, where:

- **MCP Hosts** run the AI models
- **MCP Clients** initiate requests
- **MCP Servers** serve context, tools, and capabilities

### **Key Components:**

- **Resources** – Static or dynamic data for models  
- **Prompts** – Predefined workflows for guided generation  
- **Tools** – Executable functions like search, calculations  
- **Sampling** – Agentic behavior via recursive interactions

### How MCP Servers Work

MCP servers operate in the following way:

- **Request Flow**:
    1. A request is initiated by an end user or software acting on their behalf.
    2. The **MCP Client** sends the request to an **MCP Host**, which manages the AI Model runtime.
    3. The **AI Model** receives the user prompt and may request access to external tools or data via one or more tool calls.
    4. The **MCP Host**, not the model directly, communicates with the appropriate **MCP Server(s)** using the standardized protocol.
- **MCP Host Functionality**:
    - **Tool Registry**: Maintains a catalog of available tools and their capabilities.
    - **Authentication**: Verifies permissions for tool access.
    - **Request Handler**: Processes incoming tool requests from the model.
    - **Response Formatter**: Structures tool outputs in a format the model can understand.
- **MCP Server Execution**:
    - The **MCP Host** routes tool calls to one or more **MCP Servers**, each exposing specialized functions (e.g., search, calculations, database queries).
    - The **MCP Servers** perform their respective operations and return results to the **MCP Host** in a consistent format.
    - The **MCP Host** formats and relays these results to the **AI Model**.
- **Response Completion**:
    - The **AI Model** incorporates the tool outputs into a final response.
    - The **MCP Host** sends this response back to the **MCP Client**, which delivers it to the end user or calling software.

---

## 2. Prerequisites & Environment Setup

Create and activate a virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
```

Install MCP Python SDK (includes CLI):

```bash
pip3 install "mcp[cli]"
```

---

## 3. Building Your First MCP Server

Create `server.py` inside the lab-06-mcp folder.

```python
from mcp.server.fastmcp import FastMCP

# Create an MCP server instance
mcp = FastMCP("Demo")
```

At this point the server has a name but exposes no tools or resources.

---

## 4. Expanding the Server: Tools & Resource

Add an initial math tool:

```python
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b
```

Add more tools like substract, multiply and divide.

Add a dynamic resource:

```python
@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """Return a personalized greeting"""
    return f"Hello, {name}!"
```

## 5. Running & Inspecting Your Server

Basic run:

```bash
mcp run server.py
```

Dev mode (auto Inspector + session token):

```bash
mcp dev server.py
```

---

## 7. Writing a Python MCP Client

Create `client.py`. This client:

Key imports:

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client
```

Define how to start the server:

```python
server_params = StdioServerParameters(
    command="mcp",
    args=["run", "server.py"],
    env=None,
)
```

This doesn't do run the MCP server yet, for that we need to call it.

---

## 8. Full Client Example

```python

async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()

            print("== TOOLS ==")
            tools = await session.list_tools()
            for t in tools.tools:
                print(" -", t.name)

            print("\n== RESOURCES ==")
            resources = await session.list_resources()
            for r in resources:
                print(" -", r)

            print("\n== GREETINGS ==")
            for name in ["hello", "Alice", "Bob"]:
                try:
                    content, mime = await session.read_resource(f"greeting://{name}")
                    print(f"{name}: {content}")
                except Exception as e:
                    print(f"Failed greeting {name}: {e}")

            print("\n== TOOL CALLS ==")
            calls = [
                ("add", {"a": 1, "b": 7}),
                ("multiply", {"a": 3, "b": 9}),
                ("divide", {"a": 42, "b": 7}),
                ("power", {"base": 2, "exp": 8}),
            ]
            for tool, args in calls:
                result = await session.call_tool(tool, arguments=args)
                print(f"{tool} {args} => {result.content}")

            print("\n== ERROR DEMO (divide by zero) ==")
            try:
                await session.call_tool("divide", arguments={"a": 1, "b": 0})
            except Exception as e:
                print("Expected error:", e)

if __name__ == "__main__":
    import asyncio
    asyncio.run(run())
```

Run and see the tools you have access too.

```bash
python3 client.py
```

---

## 9. Using Your Server in GitHub Copilot Agent Mode

This lets Github Copilot Chat auto‑discover and invoke your MCP server as a tool source inside the editor.

### 9.1 What the `.vscode` Folder Is (and Why It Matters)

The `.vscode` folder:
- Lives at the *root of the workspace* (same level as `lab-06-mcp/`).
- Applies settings only to THIS project (not global VS Code settings).
- Is where Copilot (in Agent Mode) looks for per‑workspace MCP configuration.
- Can safely be committed (unless it contains secrets—avoid that).

You’ll add:
1. `settings.json` – turns on MCP discovery
2. `mcp.json` – declares one or more MCP servers (your custom one + any external MCPs, like Playwright)

> Tip: If MCP discovery “does nothing,” confirm you didn’t accidentally name the file `setting.json` or put it in `lab-06-mcp/.vscode/` instead of the repo root.

### 9.2 `settings.json` (Enable Discovery)

Create (or open) `.vscode/settings.json`:

```jsonc
{
  // Enable experimental MCP auto-discovery for Copilot Chat
  "chat.mcp.discovery.enabled": true
}
```

If you already had a settings file, just add that key (valid JSON—no trailing commas).

### 9.3 Understanding `mcp.json`

Create `.vscode/mcp.json`. This file is a registry of MCP servers the Copilot host can spawn. Structure:

```jsonc
{
  "servers": {
    "<logical-name>": {
      "type": "stdio",
      "command": "<executable to launch>",
      "args": ["<arg1>", "<arg2>", "..."],
      "env": {
        // Optional: key/value environment variables (strings only)
      }
    }
  },
  "inputs": []
}
```

### 9.4 Minimal Example (Your Custom Server)

If you installed the MCP Python SDK into a virtual environment named `.venv`:

**macOS / Linux:**
```jsonc
{
  "servers": {
    "first-mcp": {
      "type": "stdio",
      "command": "${workspaceFolder}/.venv/bin/python",
      "args": ["lab-06-mcp/server.py"]
    }
  },
  "inputs": []
}
```

> Why not just `"python"`? Using the full path ensures the correct virtual environment interpreter even if your global PATH points somewhere else.

### 9.5 Start from VS Code

1. Open the mcp.json file.
2. Click the play ▶ icon next to `first-mcp`.
3. Open Copilot Chat and type: “Add 22 to 1 using the MCP server please”.
4. You should see the `add` tool invoked → response: `23`.

### 9.6 Try More Prompts

- “multiply 7 by 6”
- “divide 42 by 7”
- “power 2 to 8”
- “greet Maria”

If a tool doesn’t trigger: make the intent clearer (“Use the add tool to add 5 and 11”).

---

# Lab 06 (Part 2) – Using already created MCPs

## Lab 06 (Part A) – Playwright MCP

This lab guides you through using a Playwright Model Context Protocol (MCP) integration (via GitHub Copilot Chat in Agent mode) to:
1. Open the NBA standings page and capture a screenshot (Goal 1).
2. Extract regular season standings into structured JSON (Goal 2).
3. Enrich the top teams with extra details from their team pages (Goal 3).

---

### 🚀 Getting Started - Install the Playwright MCP 

Navigate to `https://github.com/mcp/microsoft/playwright-mcp` and follow the steps to install the Playwright MCP depending on preferred IDE.

For VS Code, the Playwright MCP server is configured in `.vscode/mcp.json`:

```json
{
  "servers": {
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

Click "Start" next to the `playwright` server in the mcp.json file to launch it.

---

### Explore MCP offering

https://github.com/mcp

### 🎯 Goal 1 – Capture Standings

Ask the Agent to Navigate to `https://www.nba.com/standings`, handle consent, scroll fully, and save a full-page screenshot with the PlayWright MCP, add more details like browser to use and such.

<details>
  <summary><strong>Show Prompt for Goal 1</strong></summary>

```text
Use the Playwright MCP with Microsoft Edge (headed). Go to https://www.nba.com/standings.
Maximize window.
Wait for network idle and the main standings table to be visible.
If a cookie/consent banner appears, accept or close it.
Scroll the full page to trigger any lazy-loaded content.
Return a short status JSON with:
{
  "ok": true/false,
  "title": "<page title>",
  "url": "<final url>",
  "detectedTables": <count>,
  "screenshot": "<path-to-screenshot>"
}
Also save a full-page PNG screenshot as ./artifacts/01_open_standings.png and include its path.
```
</details>

---

### 📊 Goal 2 – Extract Regular Season Standings

Objective: Convert the visible standings table(s) into a normalized JSON array, ask the Playwright MCP to get get the standings in a JSON format.

<details>
  <summary><strong>Show Prompt for Goal 2</strong></summary>

```text
From https://www.nba.com/standings in the current session, extract the full regular season standings table into a clean JSON array.

Requirements:
One JSON object per team with fields:
{ "rank", "team", "conference", "wins", "losses", "winPct", "gamesBehind", "streak", "home", "away", "last10" }
Normalize team names (e.g., "L.A. Clippers" → "LA Clippers"). Parse numbers as numbers.
If the site separates by conferences, include the correct "conference" value.
Validate: no empty rows, rank is numeric and unique within a conference.

Output:
Return the JSON array (pretty-printed).
Save it to ./data/standings_raw.json and confirm file path in your reply.
```
</details>

---

### 🏟️ Goal 3 – Enrich Top Teams with Extra Details

Objective: For the top 4 teams per conference, navigate to each official NBA team page, extract one reliable attribute (arena OR coach; fallback to page URL), and merge back into enriched data.

You may first create `standings_enriched.json` (can just copy or augment `standings_raw.json`) if required by your MCP flow—some tools want a separate input file.

<details>
  <summary><strong>Show Prompt for Goal 3</strong></summary>

```text
Using ./data/standings_enriched.json:

For the top 4 teams per conference, navigate to their official team page from nba.com (e.g., via the Teams directory page).
For each team, attempt to extract a visible attribute to demo multi-page scraping (choose one you can reliably find):
"arena" name or "coach" name; if not found, collect "team page URL".
Add this field as teamDetail for each of the visited teams.

Return a compact JSON report with:
{
  "visitedTeams": <count>,
  "details": [ { "team": "...", "teamDetail": "...", "url": "..." } ],
  "notes": "any selectors used or fallbacks"
}

Also save merged data to ./data/standings_enriched_with_details.json.
```
</details>

---

## Lab 06 (Part B) – Using the Microsoft Learn MCP

This part shows how to use the Microsoft Learn MCP inside GitHub Copilot (Agent Mode) to research official Microsoft documentation with focused natural‑language prompts.

---

### 1. Install the Microsoft Learn MCP Server

Follow the installation instructions for your IDE here:

https://github.com/mcp/microsoftdocs/mcp#-installation--getting-started

### 2. Open Copilot Chat (Agent Mode)

1. Confirm the Microsoft Learn MCP server is running.
2. Open a new Copilot Chat session.
3. Use the prompt examples below—these are crafted to:
   - Force authoritative source usage.
   - Encourage deeper summaries.
   - Retrieve structured or multi‑section answers.

---

### 3. Prompt Set (Try These)

Use each prompt as-is first; then refine or chain follow‑ups.

#### Prompt 1 – End-to-End MCP Understanding
```
I need to understand MCP in GitHub Copilot end-to-end (agent mode, registries, server setup). search Microsoft docs.
```

#### Prompt 2 – Core Copilot Features by IDE
```
List the core GitHub Copilot features and which IDEs they work in. deep dive
```

#### Prompt 3 – Azure Functions (Full Lifecycle)
```
I need to understand Azure Functions end-to-end. search Microsoft docs and deep dive
```

#### Prompt 4 – How GitHub Copilot Works
```
How does GitHub Copilot work, search in Microsoft Docs.
```

---

### 4. Enhancement & Follow‑Up Ideas

After an initial answer, try follow‑ups like:

- “Summarize that into a 5‑bullet executive brief. fetch full doc where needed.”
- “Provide a comparison table of Copilot feature support across VS Code, JetBrains, and Visual Studio.”
- “List common Azure Functions triggers with one‑line use cases. search Microsoft docs.”
- “Cite the exact Microsoft Docs URLs used.”

---

### 5. Tips for Higher-Quality Responses

| Goal | Add These Phrases |
|------|-------------------|
| Force official sources | `search Microsoft docs`, `fetch full doc` |
| Depth / architecture | `deep dive`, `implementation details` |
| Validation | `cite sources`, `list doc URLs` |
| Structure | `return as JSON`, `provide a table`, `outline first` |

---
