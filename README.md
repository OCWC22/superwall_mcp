# Superwall MCP Server

**Version:** 0.1.0 (In Development)

## Overview

This project provides a Model-Context-Protocol (MCP) server designed to interact with the Superwall platform. It acts as a bridge, enabling Large Language Models (LLMs) within compatible IDEs (like Cursor, Windsurf) to manage Superwall configurations (initially Paywalls) using natural language commands via the Superwall Management API.

This allows developers to streamline their workflow by managing Superwall Paywalls directly from their development environment.

## Features (Planned - MVP)

*   **Paywall Management:** List, create, read details of, update, and delete Superwall Paywalls.
*   **MCP Standard:** Communicates using the MCP standard.
*   **Secure API Key Handling:** Uses environment variables for Superwall API keys.

## Project Status

This project is currently in the initial setup phase. Core features are planned but not yet implemented.

## Setup

1.  **Prerequisites:**
    *   Node.js (v18 or higher recommended)
    *   npm
    *   Access to a Superwall account and a Management API Key.

2.  **Clone the repository (if applicable):**
    ```bash
    git clone <repository-url>
    cd superwall-mcp-server
    ```

3.  **Install Dependencies:**
    ```bash
    npm install
    ```

4.  **Configure Environment Variables:**
    *   Create a `.env` file in the project root directory by copying the example file:
        ```bash
        cp .env.example .env
        ```
    *   Edit the `.env` file and add your Superwall Management API Key:
        ```dotenv
        SUPERWALL_API_KEY=your_superwall_api_key_here
        # Add any other required environment variables here (e.g., API base URL if needed)
        ```

## How to Run

*   **Development Mode (with hot-reloading):**
    ```bash
    npm run start:dev
    ```
    This uses `nodemon` and `ts-node` to automatically restart the server on file changes.

*   **Production Mode:**
    1.  Build the TypeScript code:
        ```bash
        npm run build
        ```
    2.  Run the compiled JavaScript code:
        ```bash
        npm run start:prod
        ```

## Available MCP Tools (Planned)

_(Note: These tools will be implemented in Phase 3. Details are preliminary.)_

*   `list_paywalls`:
    *   **Description:** Retrieves a list of all Paywalls in your Superwall account.
    *   **Input:** None
    *   **Output:** A list of Paywall objects (containing ID, name, etc.).
*   `get_paywall_details`:
    *   **Description:** Gets the detailed configuration for a specific Paywall.
    *   **Input:** `{ "paywall_id": "<paywall-uuid>" }` (or potentially name)
    *   **Output:** Detailed Paywall configuration object.
*   `create_paywall`:
    *   **Description:** Creates a new basic Paywall.
    *   **Input:** `{ "name": "New Paywall Name", "identifier": "unique_identifier", ... }` (Specific fields TBD based on API)
    *   **Output:** The newly created Paywall object.
*   `update_paywall`:
    *   **Description:** Updates specific fields of an existing Paywall.
    *   **Input:** `{ "paywall_id": "<paywall-uuid>", "updates": { "field_to_update": "new_value", ... } }` (Specific fields TBD)
    *   **Output:** The updated Paywall object.
*   `delete_paywall`:
    *   **Description:** Deletes or archives a specific Paywall.
    *   **Input:** `{ "paywall_id": "<paywall-uuid>" }`
    *   **Output:** Confirmation message.

## Contributing

(Optional: Add contribution guidelines if this becomes an open project).

## License

(Optional: Specify a license, e.g., MIT). 