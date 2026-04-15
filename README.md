# Arkvion

🚀 **Live Demo:** [https://arkvion.onrender.com/](https://arkvion.onrender.com/)

Arkvion is a comprehensive web-based financial tracker integrated with an AI assistant powered by RAG (Retrieval-Augmented Generation). 

Developed for INFO2602, this application is built upon a custom FastAPI foundation (inspired by the [full-stack FastAPI template](https://github.com/fastapi/full-stack-fastapi-template)) that utilizes a strict, layered architecture combining the Model-View-Controller (MVC) and Service Repository patterns. 

This codebase is structured to enforce the DRY (Don't Repeat Yourself) principle. By deduplicating code across our data access, business logic, and presentation layers, Arkvion remains easy to maintain, test, and scale—whether functioning as a headless API, a full-stack web app, or a CLI tool.

## How the Application Flows

Arkvion follows an API-first, modern AJAX flow:
1. **Views:** The application can render the user interface directly via backend view routes.
2. **AJAX Interception:** A small JavaScript script (`app/static/js/utils.js`) intercepts default form submissions from the UI and securely routes the data to the appropriate backend **API** endpoints.

Because of this decoupled structure:
* The backend API can easily be consumed by mobile or desktop apps in the future.
* The API endpoints seamlessly support integration with external AI agents and function-calling features.

---

## 🛠️ Getting Started

Follow these steps to set up and run Arkvion locally on your machine.

**1. Clone the repository**
```bash
git clone [https://github.com/your-username/Arkvion.git](https://github.com/your-username/Arkvion.git)
cd Arkvion
```

**2. Create a virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate
```

**3. Install dependencies**
```bash
pip install .
```

**4. Configure your environment**
You **MUST** set up your environment variables before launching the application. Copy the provided template to create your active environment file:
```bash
cp env.example .env
```
> **Note:** The preconfigured `.env` allows the app to run out-of-the-box using a local SQLite database for standard data storage. Ensure any AI/RAG-specific variables (like database paths or API keys) are also configured here. 

**5. Run the application**
Start the development server using Uvicorn:
```bash
uvicorn app.main:app --reload
```

**6. Access the App**
* **Web UI:** Open your browser to `http://localhost:8000`
* **API Docs:** View the interactive Swagger UI at `http://localhost:8000/docs`

---

## Architecture: MVC & Service Repository Patterns

Arkvion's backend is intentionally separated into distinct layers to keep business logic isolated from data access. 

### The MVC Breakdown
- **Models:** SQLModel/SQLAlchemy classes that define our database tables.
- **Controllers:** Utility functions used to mutate models and execute core logic.
- **Views:** Bind controllers to HTTP routes, passing user requests to the appropriate services.

### The Service Repository Breakdown
- **The Repository Layer:** Acts as a mediator between the application and the data source (e.g., the relational database or vector databases like ChromaDB). Its only job is to handle **CRUD** operations. It doesn't care about business rules.
- **The Service Layer:** Sits between the controller and the repository. This is where Arkvion's business logic lives, handling tasks like authorization checks, data formatting, and AI prompt orchestration before interacting with the repository.

---

## App Structure

Arkvion's codebase is organized as follows:

```text
Arkvion
|-- app
|   |-- api             # Headless API endpoints
|   |-- dependencies    # Dependency injection (e.g., auth, db sessions)
|   |-- models          # Database table definitions
|   |-- repositories    # Data access and CRUD operations
|   |-- schemas         # Pydantic models for data validation 
|   |-- services        # Core business logic and rules
|   |-- static          # CSS, JS, images, and frontend assets
|   |-- templates       # Jinja2 HTML templates
|   |-- utilities       # Generic helper functions
|   |-- views           # Routes for rendering the frontend UI
|-- env.example         # Template for environment variables
|-- pyproject.toml      # Project metadata and dependencies
|-- README.md
```

---

## Deployment Notes

If you are deploying Arkvion to a production environment (like Render, Azure, etc.), ensure the following are addressed:

1. **Environment Configuration:** Update the `.env` variables to use a production-ready database (e.g., PostgreSQL) and generate a secure, unique secret key.
2. **Database Migrations:** Utilize a migration tool like Alembic to manage schema changes in production.
3. **Security:** Consider switching from local storage to HTTP-only, secure cookies for session management based on your specific security constraints.
