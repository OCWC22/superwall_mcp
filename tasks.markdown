# Superwall MCP Server - Product Requirements Document (PRD) & Tasks

**Version:** 1.0
**Date:** 2025-03-18

## 1. Overview

This document outlines the requirements and tasks for building a Model-Context-Protocol (MCP) server designed to interact with the Superwall platform. The server will act as a bridge, enabling Large Language Models (LLMs) within IDEs (like Cursor, Windsurf) to manage Superwall configurations (Paywalls, potentially Campaigns) via the Superwall Management API.

## 2. Goals

- **Enable IDE-based Superwall Management:** Allow developers to create, read, update, and delete (CRUD) Superwall Paywalls and potentially Campaigns directly from their IDE using natural language commands interpreted by an LLM.
- **Improve Developer Workflow:** Streamline the process of managing Superwall configurations by reducing the need to switch contexts between the IDE and the Superwall web dashboard.
- **Standardized Interaction:** Utilize the MCP standard for communication between the LLM and the Superwall API.
- **Secure API Interaction:** Ensure secure handling of Superwall API credentials.

## 3. Architecture

- **Technology Stack:** Node.js, TypeScript, `@modelcontextprotocol/sdk`, `axios` (or similar for HTTP requests), `dotenv`, `zod` (for validation).
- **Core Components:**
    - **MCP Server:** The main application running Node.js.
    - **MCP Transport:** Standard I/O (or other configured transport) for communication with the LLM.
    - **Tool Definitions:** TypeScript functions exposed via MCP that map LLM requests to Superwall API actions (e.g., `createPaywall`, `listPaywalls`, `updatePaywallConfiguration`).
    - **Superwall API Service:** A module responsible for making authenticated calls to the Superwall Management API.
    - **Configuration:** Environment variables (`.env`) for API keys and other settings.
    - **Models/Schemas:** TypeScript types and Zod schemas for validating API requests/responses.
- **Deployment:** Initially run locally, potentially containerized with Docker for easier distribution/management later.

## 4. Features (MVP - Paywalls)

- **Authentication:** Securely configure and use Superwall API Keys.
- **Paywall Listing:**
    - List all available Paywalls.
    - Filter Paywalls (e.g., by name, status - if API supports).
- **Paywall Creation:**
    - Create a new Paywall (potentially from a template or basic structure). Define necessary parameters (name, identifier, basic configuration).
- **Paywall Reading:**
    - Get detailed configuration for a specific Paywall (by ID or name).
- **Paywall Updating:**
    - Modify specific configurations of an existing Paywall (e.g., update text, button actions - specifics depend heavily on API capabilities).
- **Paywall Deletion/Archiving:**
    - Delete or archive a Paywall (based on API support).

## 5. Potential Future Features (Post-MVP)

- **Campaign Management:** Implement CRUD operations for Superwall Campaigns.
- **Template Management:** List and potentially use Paywall templates.
- **Advanced Filtering/Searching:** More sophisticated querying of Paywalls/Campaigns.
- **Configuration Validation:** Add more robust validation against Superwall's rules before sending API requests.
- **Deployment Integration:** Tools to potentially trigger deployments or updates within Superwall.

## 6. Non-Goals (MVP)

- **UI:** This is a backend server, no graphical user interface.
- **Complex State Management:** The server will be largely stateless, relying on the Superwall API as the source of truth.
- **Real-time Updates:** The server responds to specific LLM requests; it doesn't monitor Superwall for changes proactively.
- **User Management:** Assumes a single Superwall API key context.

## 7. Open Questions/Assumptions

- **Superwall Management API Capabilities:** The exact features depend heavily on the available Superwall Management API endpoints and their request/response structures. Documentation review is critical. (Assumption: A functional Management API exists for CRUD operations on Paywalls).
- **API Authentication Method:** Assuming API Key based authentication.
- **IDE/LLM Integration:** Assumes the IDE's LLM can communicate via a standard MCP transport.

## 8. Task Breakdown

### Phase 1: Project Setup & Foundation (Estimate: ~2-4 hours)

-   [ ] **Task 1.1:** Initialize Node.js project (`npm init`).
-   [ ] **Task 1.2:** Install core dependencies (`typescript`, `@types/node`, `ts-node`, `nodemon`, `dotenv`, `@modelcontextprotocol/sdk`, `axios`, `zod`).
-   [ ] **Task 1.3:** Configure TypeScript (`tsconfig.json`).
-   [ ] **Task 1.4:** Set up project structure (e.g., `src/`, `src/controllers`, `src/services`, `src/models`, `src/utils`).
-   [ ] **Task 1.5:** Configure basic linting/formatting (ESLint, Prettier).
-   [ ] **Task 1.6:** Create `.env.example` and configure basic environment variable loading (`dotenv`).
-   [ ] **Task 1.7:** Set up `package.json` scripts (`build`, `start:dev`, `start:prod`).
-   [ ] **Task 1.8:** Create `coding_updates/coding_updates_1.md` for tracking changes.

### Phase 2: Superwall API Integration & Service Layer (Estimate: ~4-8 hours)

-   [ ] **Task 2.1:** Research Superwall Management API documentation for Paywall CRUD endpoints.
    -   [ ] Identify endpoints for List, Get, Create, Update, Delete Paywalls.
    -   [ ] Understand authentication requirements (API Key location, headers).
    -   [ ] Analyze request/response formats.
-   [ ] **Task 2.2:** Create Superwall API base service (`src/services/superwallApiService.ts`).
    -   [ ] Implement base request logic with `axios` (or similar).
    -   [ ] Handle base URL and common headers.
    -   [ ] Implement API key authentication logic (reading from `.env`).
    -   [ ] Basic error handling for API requests.
-   [ ] **Task 2.3:** Define core data models/types (`src/models/types.ts`) based on API documentation (e.g., `Paywall`, `PaywallConfiguration`).
-   [ ] **Task 2.4:** Define Zod schemas (`src/models/schemas.ts`) for validating key API request/response structures related to Paywalls.
-   [ ] **Task 2.5:** Implement service methods for Paywall CRUD operations in `superwallApiService.ts`.
    -   [ ] `listPaywalls()`
    -   [ ] `getPaywall(id: string)`
    -   [ ] `createPaywall(payload: CreatePaywallInput)`
    -   [ ] `updatePaywall(id: string, payload: UpdatePaywallInput)`
    -   [ ] `deletePaywall(id: string)`
    -   *(Method names and parameters might change based on API specifics)*
-   [ ] **Task 2.6:** Implement basic unit/integration tests for the `superwallApiService` (using Jest or similar).

### Phase 3: MCP Controller & Tool Definitions (Estimate: ~4-6 hours)

-   [ ] **Task 3.1:** Create utility for defining MCP tools (`src/utils/defineTool.ts`) - similar to the ClickUp example.
-   [ ] **Task 3.2:** Create utility for standardized MCP error handling (`src/utils/errors.ts`).
-   [ ] **Task 3.3:** Create the main MCP controller (`src/controllers/superwallController.ts`).
-   [ ] **Task 3.4:** Implement MCP tool definitions within the controller, mapping to the service methods:
    -   [ ] `list_paywalls` tool (calls `superwallApiService.listPaywalls`).
    -   [ ] `get_paywall_details` tool (calls `superwallApiService.getPaywall`). Define input schema (e.g., `paywall_id` or `paywall_name`).
    -   [ ] `create_paywall` tool (calls `superwallApiService.createPaywall`). Define input schema for necessary parameters (name, etc.).
    -   [ ] `update_paywall` tool (calls `superwallApiService.updatePaywall`). Define input schema (ID, fields to update).
    -   [ ] `delete_paywall` tool (calls `superwallApiService.deletePaywall`). Define input schema (ID).
    -   *(Ensure clear descriptions and input/output schemas (`zod`) for each tool)*
-   [ ] **Task 3.5:** Implement the main server entry point (`src/index.ts`).
    -   [ ] Initialize MCP server instance.
    -   [ ] Register tools from the controller.
    -   [ ] Connect to the standard I/O transport.
    -   [ ] Add basic logging.

### Phase 4: Testing & Documentation (Estimate: ~3-5 hours)

-   [ ] **Task 4.1:** Perform manual testing using the MCP inspector (if available/applicable) or simulated inputs.
-   [ ] **Task 4.2:** Write integration tests covering the flow from MCP tool call to API interaction.
-   [ ] **Task 4.3:** Refine error handling and logging based on testing.
-   [ ] **Task 4.4:** Create a `README.md` file.
    -   [ ] Project description.
    -   [ ] Setup instructions (dependencies, environment variables).
    -   [ ] How to run (dev, prod).
    -   [ ] Overview of available MCP tools and their usage.
-   [ ] **Task 4.5:** Update `coding_updates_1.md` with all changes made.

### Phase 5: Deployment & Refinement (Optional/Future)

-   [ ] **Task 5.1:** Create a `Dockerfile` for containerization.
-   [ ] **Task 5.2:** Build and test the Docker image.
-   [ ] **Task 5.3:** Consider deployment options (e.g., local Docker, cloud service).
-   [ ] **Task 5.4:** Gather feedback from usage and refine tools/features.
-   [ ] **Task 5.5:** Begin implementation of Campaign Management features (if desired).

---
**Estimated Total MVP Time:** ~13 - 23 hours (highly dependent on API complexity) 