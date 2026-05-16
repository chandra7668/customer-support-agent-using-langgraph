# Customer Support Agent (Gemini)

A small LangGraph-based customer support workflow that uses Google Gemini (via `google-genai`) to categorize queries, analyze sentiment, and generate responses.

**Contents**
- **Purpose:** A notebook-driven example that demonstrates routing customer queries to handlers (technical, billing, general) and escalating negative sentiment.
- **Notebook:** `customersupport.ipynb`
- **Dependencies:** See `requirements.txt` (includes `google-genai`).

**Prerequisites**
- Python 3.10+ and a virtual environment.
- A Google Gemini API key with access to the Generative Language API.

1. Create and activate a virtual environment:

```
python -m venv venv
venv\Scripts\activate    # Windows
source venv/bin/activate  # macOS / Linux
```

2. Install dependencies:

```
pip install -r requirements.txt
```

3. Add your Gemini API key to a `.env` file in the project root:

```
gemini_API_KEY="YOUR_GEMINI_API_KEY_HERE"
```

How to run
- Open `customersupport.ipynb` in VS Code or Jupyter and run cells in order.
- The main entrypoint is the `run_customer_support(query)` helper inside the notebook.

Gemini model and quotas
- The notebook currently calls `models/gemini-2.5-flash` via `google-genai`.
- The free tier has tight per-minute request limits. If you see a `429 RESOURCE_EXHAUSTED` error, either:
  - Wait the suggested retry delay (message shows how long to wait), or
  - Reduce the number of model calls (see "Reducing API calls"), or
  - Upgrade your Google Cloud / Gemini plan.

Reducing API calls (optional)
- The workflow currently makes three model calls per query (categorize, sentiment, handler). To save quota, consider combining steps into a single prompt that returns category, sentiment, and response in one call. If you want, I can patch the notebook to use a single-call flow.

Retry handling
- The notebook includes a retry helper around the Gemini calls that will retry on 429 with a small backoff. This helps transient quota bursts but will not bypass genuine quota limits.

Troubleshooting
- `gemini_API_KEY` missing: confirm `.env` is present and loaded, and restart the kernel.
- `ModuleNotFoundError: google.genai`: ensure `google-genai` is installed in the active environment.
- `models/... not found`: call `client.models.list()` (in the notebook) to discover available models, and update the model name.

Next steps (suggested)
- Patch the notebook to use a single Gemini call per query to reduce API usage. Reply "single-call" and I will implement it.
- Add a lightweight CLI or Flask wrapper to expose the agent as an HTTP endpoint.

License
- MIT-style: use and modify freely.

Contact
- If you want the single-call reduction or help deploying, tell me which option you'd like.