Your task is to create a complete, robust URL shortener web application with usage tracking and enhanced analytics, designed to run within an Eclipse Che DevWorkspace on OpenShift.

**Project Objective:**
Build a full-stack application that allows a user to input a long URL and receive a new, unique short link every time. The application will use a simple database to store these links and track usage data. When a user navigates to the short link, they should be redirected to the original URL. All chosen dependencies and packages for both frontend and backend must be actively maintained and not marked as deprecated.

**Development Workflow:**
* Initialize a Git repository at the very beginning of the project if there is none.
* Create a comprehensive `.gitignore` file at the project root, if not found. It must ignore standard directories (`node_modules`, `build`), environment files (`.env*`), and temporary tool files like the `.ra-aid*/` directory and `aider*` files.

**Backend Requirements (Node.js):**
1.  **Framework:** Use Express.js.
2.  **Language:** Use TypeScript.
3.  **Data Persistence & Schema:**
    - **URLs**:
        * Use a simple file-based JSON database (e.g., `db.urls.json`) for persistence.
        * Each record in the JSON database should have the following structure: `{ "shortCode": "string", "longUrl": "string", "usageCount": number, "createdAt": "string" }`.
    - **Words**:
        * Use a simple file-based JSON database (e.g., `db.words.json`) for persistence.
        * Each record in the JSON database should have the following structure: `{ "word": "string", "partOfSpeach": "string", "usageCount": "number" }`.
            * `word` can be any noun or ajective.
            * `partOfSpeach` means a part of speach: "noun", "adj".
            * `usageCount` shows the number of usages.
4.  **Core Functionality:**
    * **`db.words.json` file**: Fill this DB with at least 100 adjectives and 100 nouns for use in shortCode generation.
    * **`POST /api/shorten` Endpoint:**
        * Add validation to ensure the 'url' field exists and is a properly formatted URL. If validation fails, respond with a 400 Bad Request status code and a clear JSON error message.
        * A new, unique short link **must** be generated every time this endpoint is called.
        * Generate a new, unique `shortCode` using template `{adjective}-{noun}`. The generation logic must handle potential collisions: if a newly generated code already exists, the process must repeat until a unique code is found.
        * Create a new record in the database with the `shortCode`, `longUrl`, an initial `usageCount` of `0`, and a `createdAt` field containing the current ISO 8601 timestamp.
        * Return the `shortCode` to the client.
    * **`GET /:shortCode` Endpoint:**
        * Find the link record corresponding to the `shortCode`.
        * Increment the `usageCount` for that record and save the updated data.
        * Perform a 302 redirect to the `longUrl`.
        * If the `shortCode` is not found, respond with a 404 Not Found error.
5.  **Environment Configuration:**
    * Install and configure the `cors` package to allow requests from the frontend's dynamic URL.
6.  **Testing, Coverage & Linting:**
    * Set up Jest and Supertest. Configure Jest to generate a code coverage report.
    * Write comprehensive unit tests for all endpoints, validation, and core logic, achieving a minimum of 80% test coverage.
    * Configure ESLint with a standard ruleset.

**Frontend Requirements (React/TypeScript):**
1.  **Framework:** Use Create React App with the TypeScript template.
2.  **Environment Configuration:**
    * Create a `.env.development` file with the content: `DANGEROUSLY_DISABLE_HOST_CHECK=true`.
3.  **Core UI & User Experience:**
    * Create a simple UI with an input field for the long URL and a "Shorten" button.
    * When displaying the shortened link, it must be constructed dynamically using `window.location.origin`. Do not hardcode `localhost`.
    * Add a 'Copy to Clipboard' button next to each generated short link.
    * Maintain and display a list of all links generated during the current browser session.
4.  **API Integration:** The frontend should communicate with the backend's `/api/shorten` endpoint.
5.  **Testing and Coverage:**
    * Set up the React Testing Library environment. Configure Jest to generate a code coverage report.
    * Write comprehensive unit tests for all components and user interactions, achieving a minimum of 80% test coverage.

**Development & Tooling:**
* Initialize the project with a clear directory structure (`backend/`, `frontend/`).
* Create a root `package.json` with scripts to concurrently run both services.
* Use `yarn` (version 4) for all dependency management and script execution.
    * Use the `workspaces` feature for this monorepo.
* Provide a `README.md` explaining setup, run, and test procedures, mentioning the specific configurations for the Eclipse Che environment.
* After each distinct, successfully completed task (e.g., setting up the backend server, implementing the link generation logic, creating the frontend form), create a Git commit with a clear and descriptive message.