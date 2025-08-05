# 🤖 Smart News Chatbot

A conversational AI that finds and summarizes the latest news about any company in real-time. Built with open-source models and a user-friendly Streamlit interface.


---

## 📖 About The Project

This project is an intelligent chatbot designed to solve a common problem: getting quick, reliable, and up-to-date news about specific companies without sifting through search results or using paid APIs. The bot uses a combination of web scraping and natural language processing to deliver concise, easy-to-read summaries directly in a conversational interface.

The user can simply type a company's name, and the bot will:
1.  Search the web in real-time for relevant news articles.
2.  Filter out promotional content and duplicate links.
3.  Read and understand the content of each article.
4.  Generate a high-quality, abstractive summary.
5.  Present the results in a clean, chat-based UI.

This project was built to meet the challenge of creating a smart, agent-like system using only free and open-source tools.

---

## ✨ Key Features

- **Conversational UI:** A modern, chat-like interface for a natural user experience.
- **Real-Time News:** Fetches the latest news on-demand, not from a pre-populated database.
- **AI-Powered Summarization:** Uses a sophisticated `DistilBART` model to generate informative, human-like summaries instead of just extracting sentences.
- **Typo Correction:** Intelligently detects and suggests corrections for misspelled company names (e.g., "micrasoft" -> "Did you mean Microsoft?").
- **Multi-Company Search:** Handles requests for multiple companies at once (e.g., "Google, Tesla, NVIDIA").
- **Intent Recognition:** Differentiates between greetings, questions, and company search requests.
- **Smart Filtering:** Actively filters out duplicate links, press releases, and other non-news content to improve the quality of results.
- **User-Controlled Modes:** Features a "Company News" mode to give the user explicit control over the bot's actions.

---

## 🛠️ Built With

This project leverages several powerful open-source libraries:

- **[Streamlit](https://streamlit.io/):** For creating the interactive web application UI.
- **[Hugging Face Transformers](https://huggingface.co/docs/transformers/index):** For accessing and running the pre-trained AI summarization model.
- **[googlesearch-python](https://pypi.org/project/googlesearch-python/):** For performing real-time web searches to find news articles.
- **[newspaper3k](https://pypi.org/project/newspaper3k/):** For parsing and extracting clean article text from news websites.
- **[thefuzz](https://pypi.org/project/thefuzz/):** For fuzzy string matching to detect and correct typos.

---

# 🤖 Smart News Chatbot: System Architecture

This document outlines the architecture of the Smart News Chatbot, detailing the flow of information and the role of each component. The system is designed in a modular way, where each part has a specific responsibility.

---

## 1. Architecture Diagram

The overall flow can be visualized as a pipeline that processes the user's request in several stages:

```
     ("Company Search")
              │
              v
    ┌─────────────────────┐
    │ [2. Typo Corrector] │
    └─────────┬───────────┘
┌─────────────┴─────────────┐
│                           │
v                           v
(Ask User)                  (Proceed)
│                           │
└─────────────┬─────────────┘
              │
              v
┌────────────────────────────┐
│ [3. Information Retrieval] │
└────────────┬───────────────┘
             │
             v
┌─────────────────────────┐
│   [4. NLP Processing]   │
└────────────┬────────────┘
             │
             v
┌─────────────────────────┐
│   [5. Format & Display] │
└────────────┬────────────┘
             │
             v
┌──────────────────────────┐
│   [Backend: app.py]      │
└────────────┬─────────────┘
             │
             v
┌──────────────────────────┐
│ [Frontend: Streamlit UI] │
└────────────┬─────────────┘
             │
             v
┌──────────────────────────┐
│          [User]          │
└──────────────────────────┘
```
---

## 2. Component Breakdown

Here is a detailed explanation of each component shown in the diagram:

### **Frontend: Streamlit UI**
- **Technology:** `Streamlit`
- **Role:** This is the user interface that you interact with in your web browser. It's responsible for displaying the chat history, rendering the mode-switching buttons ("General Chat" / "Company News"), and capturing your text input.

### **Backend: `app.py`**
- **Technology:** Python
- **Role:** This is the central "brain" of the application. It orchestrates the entire process, receiving input from the UI, calling other components in the correct order, and sending the final response back to the UI to be displayed. It also manages the conversation history using Streamlit's `session_state`.

### **1. Intent Classifier**
- **Technology:** Python logic (`is_greeting` function)
- **Role:** When you enter a message, this is the first decision-making step. It analyzes your text to determine your *intent*.
  - If it recognizes a greeting (like "hello" or "good morning"), it directs the backend to provide a simple, pre-written reply.
  - If it determines the intent is to find news, it passes the request to the next stage.

### **2. Typo Corrector**
- **Technology:** `thefuzz` library
- **Role:** This component activates only for company searches. It takes your input and compares it against a pre-defined list of common company names.
  - If it finds a close match that isn't exact (e.g., you type "Micrasoft," and it matches "Microsoft"), it asks you for confirmation ("Did you mean Microsoft?").
  - If your input is an exact match or has no close alternative, it allows the process to proceed directly.

### **3. Information Retrieval**
This stage is a two-part process responsible for gathering the raw data from the web.
- **a. Web Search**
  - **Technology:** `googlesearch-python`
  - **Role:** Takes the company name and performs a real-time Google search using specific keywords (e.g., `"Tesla" company news financial stock`). It returns a list of promising URLs.
- **b. Article Scraping & Filtering**
  - **Technology:** `newspaper3k`
  - **Role:** It visits each URL found in the previous step. It filters out duplicate links and irrelevant pages (like press release sections). For valid links, it intelligently extracts only the clean, main text of the news article, ignoring ads, menus, and comments.

### **4. NLP Processing (The AI Core)**
- **Technology:** `Hugging Face Transformers` pipeline (`sshleifer/distilbart-cnn-6-6` model)
- **Role:** This is where the "magic" happens. The clean text of each news article is sent to the pre-trained AI model. The model reads and "understands" the text, then generates a new, shorter version in its own words (an abstractive summary).

### **5. Format & Display**
- **Technology:** Python logic
- **Role:** The final step. The backend collects the summaries for all the articles, formats them nicely with titles and links using Markdown, adds the "Time taken" metric, and sends this final, formatted string back to the Streamlit UI to be displayed to you in the chat window.


---

## 🚀 Getting Started

To get a local copy up and running, follow these simple steps.

### Prerequisites

You need to have Python 3.8 or higher installed on your system.

### Installation

1.  **Clone the repository:**
    ```sh
    git clone https://github.com/sarveshp1710/RAP-Hackathon.git
    cd your-repo-name
    ```

2.  **Create a virtual environment (recommended):**
    ```sh
    python -m venv venv
    venv\Scripts\activate
    ```

3.  **Install the required packages:**
    ```sh
    pip install -r requirements.txt
    ```
    *(Note: You will need to create a `requirements.txt` file containing the libraries listed in the "Built With" section.)*

    Alternatively, you can install them directly:
    ```sh
    pip install streamlit googlesearch-python newspaper3k transformers sentencepiece torch thefuzz python-Levenshtein
    ```

### Running the Application

1.  Run the Streamlit app from your terminal:
    ```sh
    streamlit run app.py
    ```
2.  Your web browser should automatically open with the chatbot interface.

---

## Usage

1.  **Select a Mode:**
    - **General Chat:** For greetings and simple interactions.
    - **Company News:** To activate the news-finding functionality.

2.  **Enter a Company Name:**
    - Type a single company name (e.g., `NVIDIA`).
    - Type multiple company names separated by commas (e.g., `Apple, Amazon, Meta`).
    - If you make a typo, the bot will ask for confirmation. Respond with `yes` or `no`.

---

## 📞 Contact

SARVESH P - [sarveshraj1712@gmail.com](mailto:sarveshraj1712@gmail.com)

Profile Link: [https://github.com/sarveshp1710](https://github.com/sarveshp1710)
