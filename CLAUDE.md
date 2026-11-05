# Claude AI Assistant Instructions: DBATU Student Management System

This document instructs Claude (via Claude Desktop or claude.ai) on how to interact with this repository and connected MCP servers.

---

## 1. Project Architecture & Live URLs

- **Repository**: `TripshikhaSingh567/dbatu-student-management-system`
- **Default Deployment Branch**: `main`
- **Frontend**:
  - Directory: `frontend/` (Static HTML5, CSS3, Vanilla ES6 JavaScript)
  - Live URL: https://dbatu-student-management-system-fro-liart.vercel.app/
  - Deployment: Automatically re-deployed by Vercel on every push to `main`.
- **Backend**:
  - Directory: `backend/` (Node.js / Express REST API)
  - Live URL: https://dbatu-student-management-system-tgsz.onrender.com/
  - Health Endpoint: `https://dbatu-student-management-system-tgsz.onrender.com/api/health`
  - Deployment: Automatically re-deployed by Render on every push to `main` (`rootDir: backend`).
- **Database**:
  - MongoDB Atlas (Cluster: `MyCluster`, Database: `student_db`, Collection: `students`)
  - Configured in Render backend via `MONGODB_URI`.
  - Accessible to Claude via MongoDB MCP server in **READ-ONLY** mode.

---

## 2. MCP Tools & Operating Protocols

Claude has access to four MCP servers:
1. **GitHub MCP Server**:
   - Primary tool for editing files and pushing commits to the repository.
   - **Target Branch**: Always commit directly to `main` (unless user explicitly requests a feature branch).
   - **Commit Convention**: Always use clear Conventional Commit messages (`feat: ...`, `fix: ...`, `chore: ...`, `refactor: ...`, `style: ...`).
2. **Vercel MCP Server**:
   - Use to monitor frontend deployments, inspect build logs, and confirm deployment status after a push.
3. **Render MCP Server**:
   - Use to check backend deployment status and view logs.
   - **Safety Boundary**: Do NOT create new services, delete services, or modify environment variables/service settings.
4. **MongoDB MCP Server**:
   - Configured in **READ-ONLY** mode.
   - Use to query collections, inspect schemas, and verify database records. Claude must never attempt mutations, deletions, or schema drops.

---

## 3. Workflow for Faculty / User Requests

When the faculty or user gives an instruction such as *"Change the title on the homepage to X"* or *"Update the styling of Y"*:

1. **Locate & Modify**: Identify the relevant files (e.g. `frontend/index.html`, `frontend/style.css`, `frontend/script.js`, or `backend/server.js`).
2. **Commit & Push**:
   - Update the file(s) via GitHub MCP server.
   - Set commit message following Conventional Commits (e.g., `feat(ui): update homepage badge text`).
   - Push directly to branch `main`.
3. **Monitor Redeployment**:
   - Check Vercel deployment status (for frontend changes) or Render deploy status (for backend changes).
   - Verify build completed successfully.
4. **Confirm Live Status**:
   - Confirm to the user that the change is live with the corresponding URL.

---

## 4. Security Requirements

- **Zero Secrets in Code**: Never commit `.env`, tokens, passwords, or credentials to git.
- **Secrets Storage**: All secrets reside in local `.env` or Claude Desktop's local config file (`claude_desktop_config.json`).
