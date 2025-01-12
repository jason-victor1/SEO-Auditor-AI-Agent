# Building an SEO Auditor AI Agent with SimplerLLM Library

## Overview of the Project
The SEO Auditor AI agent will:
- Analyze web pages.
- Provide actionable SEO insights (e.g., number of images, keyword optimization, response speed, etc.).
- Leverage **SimplerLLM** for interacting with a language model and external APIs (like RapidAPI’s SEO analyzer).

---

## Setup

### Install Required Libraries
1. **Create a virtual environment**:
   ```bash
   python -m venv seo_auditor_env
   source seo_auditor_env/bin/activate  # For MacOS/Linux
   seo_auditor_env\Scripts\activate    # For Windows
   ```

2. **Install the SimplerLLM library**:
   ```bash
   pip install simplerllm
   ```

### Create a Main Python File
- Create a file named `seo_auditor_ai_agent.py` in your project directory.

---

## Code Implementation

### Step 1: Import and Initialize SimplerLLM
```python
from simplerllm import Language

# Initialize the language model instance
llm = Language(provider="openai", model_name="gpt-4")
```

---

### Step 2: Define External Function to Analyze Web Pages
Use a website SEO analysis API (e.g., from RapidAPI) to retrieve web page data:
```python
import requests

def analyze_webpage(url):
    api_url = "https://api.example.com/seo-analyzer"  # Replace with the actual RapidAPI endpoint
    headers = {
        "Authorization": "Bearer YOUR_API_KEY",  # Replace with your API key
        "Content-Type": "application/json"
    }
    payload = {"url": url}

    response = requests.post(api_url, headers=headers, json=payload)
    if response.status_code == 200:
        return response.json()  # Parsed SEO data
    else:
        raise Exception(f"Error: {response.status_code}, {response.text}")
```

---

### Step 3: Define the AI Agent’s Actions
Create functions to interact with the external API and process data:
```python
def generate_seo_audit(url):
    seo_data = analyze_webpage(url)
    return seo_data
```

---

### Step 4: Define Prompts for AI Interaction
Create a reusable prompt structure for the SEO Auditor AI agent:
```python
def create_prompt(user_query, seo_data=None):
    prompt = f"""
    You are an SEO Auditor AI assistant. Your job is to answer SEO-related questions about web pages.

    User Query: {user_query}

    If additional data is provided, use it to generate accurate responses:
    SEO Data: {seo_data if seo_data else "None"}
    """
    return prompt
```

---

### Step 5: AI Agent Logic
Handle user queries, generate responses, and fetch web page analysis:
```python
def seo_auditor_ai_agent(user_query, url=None):
    # If a URL is provided, generate an SEO audit
    seo_data = None
    if url:
        seo_data = generate_seo_audit(url)

    # Create the prompt
    prompt = create_prompt(user_query, seo_data)

    # Generate a response using SimplerLLM
    response = llm.generate_response(messages=[{"role": "user", "content": prompt}])
    return response
```

---

### Step 6: Interactive Loop for User Input
Allow users to interact with the SEO Auditor AI agent:
```python
if __name__ == "__main__":
    print("Welcome to the SEO Auditor AI Agent!")
    while True:
        user_query = input("Enter your question (or 'exit' to quit): ")
        if user_query.lower() == "exit":
            break
        url = input("Enter the web page URL (or leave blank if not applicable): ")
        try:
            response = seo_auditor_ai_agent(user_query, url)
            print("\nAI Response:")
            print(response)
        except Exception as e:
            print(f"Error: {e}")
```

---

## Testing the Agent
Run the script:
```bash
python seo_auditor_ai_agent.py
```

### Example Interaction:
1. **User Query**: "How many images are on this web page?"  
2. **Web Page URL**: `https://example.com`  
3. **AI Response**: "This page contains 8 images."

---

## Extending the Agent

### Frontend Integration:
- Convert the script into an API using Flask or FastAPI.
- Integrate it with a WordPress or custom website for live user interaction.

### Enhance Functionality:
- Add features like keyword recommendations, meta tag analysis, or content readability scoring.

### Deployment:
- Deploy the agent on a cloud platform like AWS or Azure for scalability.

---

## Conclusion
Using **SimplerLLM**, you can streamline the development of an SEO Auditor AI agent:
- Simplify interaction with GPT models.
- Leverage external APIs for web page analysis.
- Focus on delivering actionable insights with minimal setup.
