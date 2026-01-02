# Resume-Reviewer-Agent with Job Matching using LangChain and MCP

**Resume Review Agent** is an end-to-end, LLM-powered system designed to automate resume analysis and job matching. It uses a role-agnostic, AI-driven pipeline to analyze resumes, detect relevant job roles, fetch real job postings, evaluate skill alignment, and generate a ranked, human-readable report without manual job searching or comparison.

The system is built using LangChain, OpenAI GPT-4.1-mini, and the Claude MCP (Model Context Protocol) Server, enabling natural-language interaction and tool-based orchestration.

## Repository Contents

* **`resume_reviewer.py`**: MCP server implementation containing all resume analysis, job fetching, and ranking logic.
* **`.env.example`**: Sample environment variable file (API keys not included).
* **`Sample Resume Files (optional)`**: PDF / DOCX / TXT files for testing.

## Execution Guide (Claude MCP / Local Execution)
Follow these steps to run the Resume Review Agent.

1. Environment Setup:
Ensure you have Python 3.9+ installed. Then install dependencies:
pip install langchain langchain-openai mcp python-dotenv requests PyPDF2 python-docx

2. Configure API Keys
Create a .env file in the project root and add:
OPENAI_API_KEY=your_openai_api_key
RAPIDAPI_KEY=your_rapidapi_key

RapidAPI is used for fetching live job postings via the JSearch API.

3. Resume Placement (Auto-Detection)
The system automatically detects the most recently uploaded resume from Claude Desktop’s uploads directory:
C:\Users\<username>\AppData\Roaming\Claude\Uploads\

Supported formats:
* PDF
* DOCX
* TXT
You may also explicitly override the resume path using an environment variable:
export RESUME_PATH_OVERRIDE=/path/to/resume.pdf

4. Start the MCP Server
Run the MCP server locally:
python resume_reviewer.py

You should see:
Starting MCP Server: resume-reviewer

# System Workflow
The Resume Review Agent executes the following automated pipeline:

** 1. Automatic Resume Detection & Extraction **
* Locates the most recent resume automatically
* Extracts text from PDF, DOCX, or TXT
* No manual file selection required

** 2. Automatic Role Detection (LLM-Driven) **
* Uses an LLM prompt to infer relevant job roles directly from resume content
* Avoids fixed or hardcoded role lists
* Detects up to 5 suitable roles per resume

** 3. Fetching Real Job Descriptions **
* Fetches live job postings using RapidAPI JSearch
* Retrieves:
    Job title
    Company name
    Full job description
    Apply link
    Post date
    Citizenship / clearance keywords
This ensures evaluation is based on real industry job requirements.

** 4. AI-Based Skill Extraction **
* Uses GPT-4.1-mini via LangChain
* Extracts only technical skills from each job description
* Converts unstructured text into clean, comparable skill lists

** 5. Resume vs Job Skill Matching **
For each job posting, the system computes:
* Total required skills
* Skills present in the resume
* Match percentage
* Missing skills list
The LLM then generates short, actionable feedback explaining how the candidate can improve alignment.

** 6. Full End-to-End Review Pipeline **
The main tool, full_resume_review_anyrole, orchestrates the complete process:
* Auto-detects and reads the resume
* Detects job roles using the LLM
* Fetches real job postings
* Extracts technical skills
* Computes match scores
* Detects U.S. citizenship / Green Card / security clearance requirements
* Sorts all results and selects the Top 5 job matches
* Generates a polished, human-readable report

# ** Output Format**
The final output displayed inside Claude includes:
* Job title
* Company name
* Match percentage
* Required skills
* Missing skills
* Citizenship / clearance warnings
* Direct job application links
* Concise improvement feedback
Example output:

RESUME TOP-5 MATCHES
#1: Software Engineer — Company X — 65% match
Apply link: https://...
NOTE: This job mentions US Citizen / Green Card / Clearance

# ** Claude MCP Integration**
* All functions are exposed as MCP tools using FastMCP
* Supports natural-language invocation from Claude
* Produces raw text output (no UI artifacts)
* Prevents extra assistant commentary
* Enables conversational use without programming knowledge

# ** Tech Stack **
* Python
* LangChain
* OpenAI GPT-4.1-mini
* Claude MCP (FastMCP)
* RapidAPI – JSearch
* NLP & Resume Parsing

# ** Example Demo **

Claude Chat Share Link:
🔗 https://claude.ai/share/79bfd83b-319b-40ae-90e8-54017dff097a

# ** Use Cases **
* Students and new graduates
* Job seekers applying across multiple roles
* Career advisors and mentors
* AI-driven resume and career tooling research
