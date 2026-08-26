# Invoice Processing AI

An AI-assisted invoice processing application that combines **Google Cloud Document AI**, **FastAPI**, **Streamlit**, and a conversational **LangChain agent** to extract invoice information, calculate extraction confidence, and provide invoice-related analysis.

<img width="1197" height="604" alt="image" src="https://github.com/user-attachments/assets/e1085c15-586a-41d6-8474-117d2ba8a9bc" />

## Overview

Invoice Processing AI is designed to reduce repetitive invoice data-entry and review work.

A user can upload an invoice through the Streamlit interface. The FastAPI backend sends the document to Google Document AI, extracts structured invoice information, and returns the results together with field-level confidence scores.

The project also includes an AI assistant that can work with invoice-processing tools and answer questions about invoice data, vendor activity, spending patterns, and items that may require review.

The repository contains the application layer, AI agent, document-processing logic, database components, tests, Docker-related configuration, monitoring configuration, and Kubernetes/Helm deployment configuration.

---

## What the System Does

```text
Invoice File
     │
     ▼
Streamlit Interface
     │
     ▼
FastAPI API
     │
     ▼
Google Document AI
     │
     ▼
Structured Extraction
     │
     ├── Vendor Information
     ├── Invoice Number
     ├── Invoice & Due Dates
     ├── Addresses
     ├── Amounts & Taxes
     └── Line Items
     │
     ▼
Confidence Scores
     │
     ▼
Results & Analysis
```

For conversational use, the project also provides an invoice-focused AI agent:

```text
User Question
      │
      ▼
Invoice AI Agent
      │
      ├── Process Invoice
      ├── Search Invoice Data
      └── Analyze Invoice Data
      │
      ▼
Business-oriented Response
```

---

## Key Features

### AI Invoice Extraction

The backend uses Google Cloud Document AI to process invoice documents and extract structured information.

The extraction layer handles fields including:

- Vendor/supplier name
- Vendor address
- Billing/receiver information
- Invoice number
- Invoice date
- Due date
- Total amount
- Net/subtotal amount
- Tax amount
- Currency
- Invoice line items

### Confidence Scoring

Document AI confidence values are collected for extracted entities and returned with the processing result.

The frontend displays these values as an interactive confidence chart so users can identify fields that may require additional review.

### Multiple File Formats

The Streamlit interface currently accepts:

- PDF
- PNG
- JPG
- JPEG
- TIFF
- GIF

The backend also validates supported extensions and applies a configurable maximum upload size.

### Single & Batch Processing

The frontend supports two processing modes:

- Single invoice processing
- Batch processing

Batch requests can send multiple invoice files to the FastAPI backend.

### Conversational Invoice Assistant

The project includes a LangChain-based invoice agent powered by Google Vertex AI / Gemini.

The assistant maintains conversation memory and provides commands for tasks such as:

- Processing invoices
- Searching invoices by vendor
- Finding invoices above a specified amount
- Reviewing recent invoices
- Analyzing spending patterns
- Identifying invoices that may need review

### Business Rules

The AI assistant is configured with invoice-review rules such as:

- Flag invoices above a specified approval threshold
- Highlight unusual patterns
- Identify possible duplicate-vendor situations
- Provide extraction confidence information
- Keep financial responses concise and business-focused

### Interactive Results

The Streamlit application presents:

- Processing status
- Processing time
- Extracted invoice fields
- Financial totals
- Line-item tables
- Confidence visualizations
- Processing history
- Success-rate and processing-time metrics

Line items can also be downloaded as CSV from the interface.

### API

The FastAPI service provides endpoints for invoice processing, batch processing, configuration, health checks, and the conversational agent.

Interactive API documentation is available through FastAPI when the server is running.

### Testing & Automation

The repository includes:

- Pytest tests
- Async test support
- GitHub Actions CI/CD configuration
- Code formatting checks
- Import sorting checks
- Flake8 checks
- Mypy checks
- Bandit security scanning
- Dependency vulnerability scanning
- Docker image build checks

### Deployment Configuration

The repository includes configuration for:

- Docker-based services
- Nginx
- Redis
- PostgreSQL
- Kubernetes Helm deployment
- Prometheus monitoring
- GitHub Actions

Some of these components are optional or require additional deployment files/configuration before production use.

---

## System Architecture

```text
                    ┌─────────────────────┐
                    │   Invoice Document  │
                    │ PDF / Image Files   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Streamlit Frontend  │
                    │ Upload + Results    │
                    └──────────┬──────────┘
                               │ HTTP
                               ▼
                    ┌─────────────────────┐
                    │    FastAPI API      │
                    │ Validation + Routes │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Google Document AI  │
                    │ Document Processing │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Structured Invoice  │
                    │ Data + Confidence   │
                    └──────────┬──────────┘
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
             ┌──────────────┐      ┌──────────────┐
             │ UI Results   │      │ AI Assistant │
             │ & Analytics  │      │ / Analysis   │
             └──────────────┘      └──────────────┘
```

---

## AI Agent Architecture

The conversational component is implemented around an invoice-specific agent.

```text
User
 │
 ▼
InvoiceAgent
 │
 ├── Conversation Memory
 │
 ├── Gemini / Vertex AI
 │
 └── Invoice Tools
      │
      ├── Invoice Processing
      ├── Invoice Search
      └── Invoice Analysis
```

The agent is configured with a low temperature for consistent business-oriented responses and maintains conversation history for follow-up questions.

---

## Technology Stack

| Technology | Role |
|---|---|
| Python | Core application language |
| FastAPI | Backend API |
| Uvicorn | ASGI server |
| Streamlit | Web interface |
| Google Cloud Document AI | Invoice document processing |
| Google Cloud Storage | Google Cloud storage dependency |
| LangChain | AI agent framework |
| Google Vertex AI / Gemini | Conversational AI |
| ChromaDB | Vector-store dependency used by the project |
| Sentence Transformers | Embedding-related dependency |
| SQLAlchemy | Database layer |
| Alembic | Database migration tooling |
| SQLite | Default database configuration |
| Pandas | Data processing |
| NumPy | Numerical processing |
| Scikit-learn | Machine-learning utilities |
| Joblib | Model serialization |
| Plotly | Interactive charts |
| Pillow | Image handling |
| Docker | Containerization |
| Kubernetes / Helm | Deployment configuration |
| Prometheus | Monitoring configuration |
| GitHub Actions | CI/CD automation |

---

## Project Structure

The repository currently contains the following main components:

```text
invoice-processing-ai-main/
│
├── .github/
│   ├── dependabot.yml
│   └── workflows/
│       └── ci-cd.yml
│
├── docker/
│   └── docker-composer.yaml.py
│
├── frontend/
│   └── app.py
│
├── helm/
│   ├── Chart.yaml
│   └── Values.yaml
│
├── monitoring/
│   └── prometheus.yml
│
├── src/
│   ├── agents/
│   │   ├── invoice_agent.py
│   │   └── tools.py
│   │
│   ├── api/
│   │   ├── agent_endpoints.py
│   │   └── main.py
│   │
│   ├── core/
│   │   ├── classifier.py
│   │   └── document_processor.py
│   │
│   ├── database/
│   │   ├── connection.py
│   │   └── models.py
│   │
│   └── utils/
│       └── config.py
│
├── tests/
│   ├── test_agent.py
│   └── test_chat.py
│
├── .env.example
├── .gitignore
├── .pre-commit-config.yaml
├── nginx.conf
├── requirements.txt
├── setup_project.sh
├── test_credentials.py
├── test_setup.py
└── README.md
```

---

## Requirements

For the main application, install:

- Python 3.8+
- Google Cloud project
- Google Document AI processor
- Google Cloud authentication credentials
- Git

Docker is useful for containerized development and deployment.

The repository's CI configuration also tests against Python 3.8 through 3.11.

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/hammadproject/invoice-processing-ai-main.git
cd invoice-processing-ai-main
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv
```

Activate it on Linux/macOS:

```bash
source .venv/bin/activate
```

On Windows:

```powershell
.venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a local `.env` file from the provided template:

```bash
cp .env.example .env
```

On Windows, copy `.env.example` to `.env` manually if required.

Then configure the values for your own Google Cloud project and local environment.

---

## Environment Variables

The repository provides the following configuration categories in `.env.example`:

```env
GCP_PROJECT_ID=your-project-id
GCP_LOCATION=your-location
GCP_PROCESSOR_ID=your-processor-id
GOOGLE_APPLICATION_CREDENTIALS=path/to/service-account.json

DATABASE_URL=sqlite:///./invoice_processing.db

API_HOST=0.0.0.0
API_PORT=8000
DEBUG=True

MODEL_PATH=data/models/document_classifier.pkl
MIN_CONFIDENCE_THRESHOLD=0.7

MAX_FILE_SIZE=10485760
UPLOAD_DIR=uploads
```

### Google Cloud Configuration

You need to provide:

- Google Cloud project ID
- Document AI processor location
- Document AI processor ID
- Path to a Google Cloud service-account credential file

Do not copy credentials from the repository's example configuration. Use credentials belonging to your own Google Cloud project.

### File Upload Configuration

The default maximum upload size is:

```text
10 MB
```

Supported extensions are defined by the FastAPI configuration:

```text
.pdf
.png
.jpg
.jpeg
.tiff
.gif
```

### Database Configuration

The default configuration uses SQLite:

```env
DATABASE_URL=sqlite:///./invoice_processing.db
```

SQLAlchemy and Alembic are included for database-related functionality.

---

## Google Document AI Setup

Before processing real invoices:

1. Create or select a Google Cloud project.
2. Enable the Document AI API.
3. Create an appropriate invoice/document processor.
4. Create a service account with the permissions required for Document AI.
5. Download the service-account JSON credential file.
6. Set `GOOGLE_APPLICATION_CREDENTIALS` to its local path.
7. Set the project, location, and processor ID in `.env`.

Example:

```env
GCP_PROJECT_ID=your-project-id
GCP_LOCATION=us
GCP_PROCESSOR_ID=your-processor-id
GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account.json
```

Keep the credential file outside version control.

---

## Running the Application

### Start the FastAPI Backend

From the project root:

```bash
cd src/api
python main.py
```

The API is configured to use:

```text
http://localhost:8000
```

### Start the Streamlit Frontend

Open another terminal and run:

```bash
cd frontend
streamlit run app.py
```

The Streamlit interface normally runs at:

```text
http://localhost:8501
```

The frontend communicates with the API at:

```text
http://localhost:8000
```

The API URL is currently defined in `frontend/app.py` and can be changed when running the frontend against a different backend environment.

---

## API Endpoints

The main API provides invoice-processing functionality including:

### Health Check

```http
GET /
```

Returns API status information.

### Single Invoice Processing

```http
POST /process-invoice
```

Accepts a single invoice file and returns the processing result.

### Batch Processing

```http
POST /batch-process
```

Accepts multiple invoice files for batch processing.

### Configuration

```http
GET /config
```

Returns the application's current configuration information exposed by the API.

### Agent Endpoints

The repository also contains an agent router with functionality for conversational invoice assistance, demo scenarios, and agent health checking.

When enabled through the FastAPI application, interactive API documentation is available at:

```text
http://localhost:8000/docs
```

and:

```text
http://localhost:8000/redoc
```

---

## Invoice Data Returned

The document-processing layer organizes extracted information into categories such as:

```json
{
  "vendor_info": {},
  "invoice_details": {},
  "line_items": [],
  "totals": {},
  "dates": {},
  "addresses": {}
}
```

Confidence values are returned separately for recognized entities.

Line-item tables are extracted from the document when table data is available.

---

## Streamlit Interface

The frontend provides:

- Single-file upload
- Batch processing
- Image preview
- Processing status
- Processing time
- Average confidence
- Extracted invoice information
- Financial totals
- Line-item tables
- CSV export for line items
- Confidence charts
- Processing history
- Success-rate metrics
- Average processing-time metrics

The interface is implemented in:

```text
frontend/app.py
```

---

## Docker & Deployment

The repository contains Docker-related configuration under:

```text
docker/
```

The Docker configuration describes services for:

- FastAPI
- Streamlit
- Redis
- PostgreSQL
- Nginx

The repository also includes Kubernetes/Helm configuration under:

```text
helm/
```

and monitoring configuration under:

```text
monitoring/
```

### Important

The deployment files contain environment-specific placeholders and assumptions. Review and customize them before using them in production.

Do not deploy with example passwords, placeholder hostnames, or development credentials.

---

## Kubernetes / Helm

A Helm chart is included under:

```text
helm/
```

It defines configurable components for:

- Backend service
- Frontend service
- PostgreSQL
- Redis
- Ingress
- Persistent storage
- Monitoring
- External secrets

Before deployment, review `helm/Values.yaml` and replace development/default values with secure production configuration.

---

## CI/CD

The repository contains a GitHub Actions workflow at:

```text
.github/workflows/ci-cd.yml
```

The workflow includes stages for:

- Dependency installation
- Formatting checks
- Import sorting
- Flake8 linting
- Mypy checks
- Pytest
- Coverage reporting
- Bandit security scanning
- Dependency vulnerability scanning
- Docker image builds
- Container security scanning
- Deployment placeholders

Production deployment steps require environment-specific secrets and deployment configuration.

---

## Testing

Run the available tests with:

```bash
pytest tests/ -v
```

The repository includes tests covering the invoice agent and chat-related functionality.

Additional project-level validation can be run through the CI workflow, which includes formatting, linting, type checking, security scanning, and Docker build validation.

---

## Security

Invoice documents can contain sensitive financial and business information. Protect both uploaded documents and extracted data accordingly.

### Never commit

- Google Cloud service-account JSON files
- `.env` files containing real credentials
- API keys
- Database passwords
- Production secrets
- Private invoice documents
- Customer financial information

### Recommended practices

- Use a dedicated Google Cloud service account.
- Grant only the permissions required by the application.
- Keep credentials outside the repository.
- Use environment variables or a secure secrets manager.
- Restrict CORS origins in production instead of allowing all origins.
- Replace default database passwords before deployment.
- Review Docker and Helm configuration before production use.
- Avoid logging sensitive invoice contents.
- Apply appropriate data-retention and access-control policies.

---

## Configuration & Customization

### Change the Document AI Processor

Update:

```env
GCP_PROJECT_ID=
GCP_LOCATION=
GCP_PROCESSOR_ID=
```

to point to the processor used by your Google Cloud project.

### Change the Upload Limit

Set:

```env
MAX_FILE_SIZE=
```

The value is interpreted in bytes.

### Change the Upload Directory

Set:

```env
UPLOAD_DIR=
```

to the desired local storage directory.

### Change the Confidence Threshold

The environment configuration includes:

```env
MIN_CONFIDENCE_THRESHOLD=0.7
```

This can be used by application components that apply a minimum confidence requirement.

### Customize AI Behavior

The conversational agent is implemented in:

```text
src/agents/invoice_agent.py
```

The agent's system instructions, business rules, available commands, model configuration, and conversation memory can be customized there.

### Add or Modify Agent Tools

Agent tools are defined in:

```text
src/agents/tools.py
```

This provides a central place for extending the assistant's invoice-related capabilities.

---

## Known Implementation Notes

This README intentionally describes the repository based on the code and configuration currently present.

A few deployment-related files contain placeholders or configuration assumptions, including Docker/Kubernetes settings. They should be reviewed before production deployment.

The repository also contains optional infrastructure components such as PostgreSQL, Redis, Prometheus, Nginx, and Helm configuration. Their presence does not mean every component is required for a basic local run.

The default application path for invoice extraction is the Google Document AI integration.

---

## Future Improvements

Potential improvements for a production implementation include:

- Add stronger authentication and authorization around the API.
- Add persistent invoice-processing history and reporting.
- Expand invoice validation and duplicate detection.
- Improve handling of currencies and international invoice formats.
- Add structured vendor matching and normalization.
- Add asynchronous background processing for large document batches.
- Add a production-grade job queue.
- Add more comprehensive integration tests.
- Add stronger API rate limiting and request validation.
- Improve observability with structured metrics and tracing.
- Add configurable approval workflows for high-value invoices.
- Add ERP/accounting-system integrations.
- Improve agent tool coverage for historical invoice analysis.
- Add human-review workflows for low-confidence extractions.

---

## Contributing

Contributions and improvements are welcome.

Before submitting a change:

1. Create a dedicated branch.
2. Keep changes focused and documented.
3. Add or update tests where appropriate.
4. Run the test suite.
5. Run the relevant linting and type checks.
6. Never commit credentials or private invoice data.
7. Update the README when configuration or user-facing behavior changes.

---

## License

There is currently **no `LICENSE` file present in the repository**.

Therefore, this README does not claim an open-source license for the project.

If the project is intended to be distributed under an open-source license, add the appropriate `LICENSE` file and update this section to match it.

---

## Summary

Invoice Processing AI brings together document understanding, structured extraction, confidence scoring, and conversational AI into a practical invoice-processing workflow.

Its architecture provides a foundation that can be extended from a local Streamlit/FastAPI application into a larger invoice automation platform with persistent storage, approval workflows, accounting integrations, monitoring, and scalable document processing.
