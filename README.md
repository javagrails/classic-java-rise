# ☕ Awakened Java Classic
Exposing a classic Java backend (PostgreSQL &amp; Google Calendar) to autonomous agents using Spring AI and Model Context Protocol (MCP)

> **No Rewrite Required:** Exposing a classic, deterministic Java backend (PostgreSQL & Google Calendar) to autonomous AI agents using **Spring AI** and the **Model Context Protocol (MCP)**.

Built for the **Java User Group Bangladesh (JUGBD) Conference 2026** session: *"Bringing Legacy Java into the AI Arena: Exposing Enterprise Backends to Autonomous Agents with Spring AI & MCP"*.

---

## 🎯 The Core Idea

Enterprise Java applications built over the last decade or two contain rock-solid business logic, relational databases, and external API integrations. Connecting them to modern Large Language Models (LLMs) does **not** require rewriting your backend or embedding probabilistic AI loops inside your core services.

This project demonstrates how to keep your Spring Boot application **100% deterministic, type-safe, and predictable**, while exposing its capabilities as standardized **MCP Tools** so an external **Autonomous AI Agent** (like Claude Desktop) can orchestrate complex, multi-step workflows across systems.

---

## 🏗️ Architecture Overview

```text
+-------------------------------------------------------------------+

|                     MCP Host / Autonomous Agent                   |
|                         (Claude Desktop)                          |
|                                                                   |
|  * Non-Deterministic Reasoning & Planning (ReAct Loop)            |
|  * Dynamic Tool Selection & Chaining                              |
+---------------------------------+---------------------------------+
                                  |
                         MCP (JSON-RPC / STDIO)
                                  |
+---------------------------------v---------------------------------+

|                   Awakened Java Classic Backend                   |
|                 (Spring Boot + Spring AI MCP Server)              |
|                                                                   |
|  * 100% Deterministic Java Code (Zero LLM inside the server)      |
|  * Exposes @Tool annotated service methods                        |
+----------------+---------------------------------+----------------+

                 |                                 |
                 v                                 v
+---------------------------------+ +-------------------------------+

|       Local PostgreSQL DB       | |     Google Calendar API       |
|  * Single & Multi-table Queries | |  * Create / Update / Delete   |
|  * Read-only Data Exploration   | |  * Manage Event Attendees     |
+---------------------------------+ +-------------------------------+
```

### Key Architectural Boundary
* **Claude Desktop (The Brain / Agent):** Handles natural language understanding, breaks down goals, decides which tools to call, passes outputs from one tool into another, and recovers from errors.
* **Spring AI MCP Server (The Hands / Deterministic Toolbox):** Executes strict Java methods against PostgreSQL and Google Calendar with full compile-time safety and security boundaries.

---

## ✨ Features & Exposed MCP Tools

### 1. Google Calendar Integration (`CalendarService`)
* **Create Event:** Schedule new meetings with title, description, start/end times, and time zones.
* **Update & Delete Event:** Modify existing event details or cancel events by ID.
* **Attendee Management:** Add or remove specific attendees from an existing Google Calendar event.

### 2. Local PostgreSQL Explorer (`DatabaseService`)
* **Single & Multi-Table Retrieval:** Query records and join relational tables safely.
* **Flexible Formatting:** Returns structured data that the MCP client can summarize, tabulate, or convert into reports.

---

## 🛠️ Tech Stack

* **Java:** 21+
* **Framework:** Spring Boot 3.x
* **AI Integration:** Spring AI (MCP Server Starter)
* **Database:** PostgreSQL
* **External API:** Google Calendar API (OAuth 2.0)
* **MCP Client:** Claude Desktop (STDIO transport)

---

## 🚀 Getting Started

### Prerequisites
1. **JDK 21** or higher installed.
2. **Maven** 3.9+ installed.
3. **PostgreSQL** running locally with sample tables populated.
4. **Google Cloud Project** with the Google Calendar API enabled and `credentials.json` downloaded.
5. **Claude Desktop** installed on your machine.

### 1. Clone & Build the Project
```bash
git clone https://github.com<your-username>/awakened-java-classic.git
cd awakened-java-classic
mvn clean package -DskipTests
```

### 2. Configure Application Properties
Update `src/main/resources/application.properties` with your local PostgreSQL credentials:

```properties
spring.application.name=awakened-java-classic

# Disable banner and console logs so STDIO transport stays clean for JSON-RPC
spring.main.banner-mode=off
logging.pattern.console=

# PostgreSQL Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/your_database
spring.datasource.username=your_username
spring.datasource.password=your_password

# Spring AI MCP Server Configuration
spring.ai.mcp.server.name=awakened-java-classic
spring.ai.mcp.server.version=1.0.0
```

*(Place your Google OAuth `credentials.json` inside `src/main/resources/` or point to its path in your configuration).*

### 3. Connect to Claude Desktop
Open your Claude Desktop configuration file:
* **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
* **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

Add your Spring Boot JAR as an MCP server:

```json
{
  "mcpServers": {
    "awakened-java-classic": {
      "command": "java",
      "args": [
        "-jar",
        "/ABSOLUTE/PATH/TO/awakened-java-classic/target/awakened-java-classic-0.0.1-SNAPSHOT.jar"
      ]
    }
  }
}
```
Restart **Claude Desktop**. You should see the hammer/tools icon indicating your Java tools are connected!

---

## 💬 Live Demo Prompts to Try

Once connected in Claude Desktop, try these prompts to see autonomous cross-tool orchestration in action:

1. **Database Query & Formatting:**
   > *"Fetch the list of students and their enrolled courses from our PostgreSQL database, and present it as a clean summary table."*

2. **Google Calendar Management:**
   > *"Schedule a 'Java Architecture Sync' tomorrow from 10:00 AM to 11:00 AM, and invite `alice@example.com`."*

3. **Autonomous Cross-Tool Chaining (Postgres ➔ Google Calendar):**
   > *"Retrieve the first 2 students' email addresses from the PostgreSQL database, create a 'Project Review' event on Google Calendar for Friday at 3:00 PM, and add both students as attendees."*

---

## 🔒 Production Guardrails Note

When exposing classic backends over MCP:
* Ensure `spring.main.banner-mode=off` is set when using **STDIO** transport, otherwise Spring's startup ASCII banner will corrupt the JSON-RPC stream.
* Restrict database accounts used by MCP tools to **read-only** roles unless write operations are explicitly audited.
* Never hardcode OAuth secrets or database passwords in version control.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
