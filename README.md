# TalentScout Hiring Assistant

## Project Overview

The TalentScout Hiring Assistant is an intelligent chatbot designed to streamline the initial screening process for technology placement candidates. Built for a fictional recruitment agency, "TalentScout," this chatbot collects essential candidate information and assesses technical proficiency by generating tailored questions based on the candidate's declared tech stack.

### Key Capabilities
- Collecting candidate details (name, email, phone, experience, position, location, tech stack)
- Generating 3-5 technical questions dynamically using a pre-trained language model
- Maintaining conversation context and providing a seamless user experience
- Ending the conversation gracefully with next steps

The chatbot is implemented as a web application using Streamlit, with ngrok for public access, and leverages the GPT-2 model for question generation.

## Installation Instructions

### Prerequisites
- Python 3.7+
- Git (optional for local setup)
- Google Colab account (if running in Colab)

### Local Setup
1. **Clone the Repository** (if hosted on Git):
   ```
   git clone https://github.com/your-username/talentscout-hiring-assistant.git
   cd talentscout-hiring-assistant
   ```

2. **Install Dependencies**:
   ```
   pip install streamlit pyngrok transformers torch
   ```

3. **Get ngrok Auth Token**:
   - Sign up at [ngrok.com](https://ngrok.com)
   - Obtain your free auth token and note it down

4. **Run the Application**:
   - Save the code as `app.py`
   - Replace `YOUR_NGROK_AUTH_TOKEN` in the code with your token
   - Start the app:
     ```
     streamlit run app.py --server.port 8501
     ```
   - In a separate terminal, expose it with ngrok:
     ```
     ngrok http 8501
     ```
   - Access the app via the ngrok URL provided

### Google Colab Setup
1. **Open Colab**: Create a new notebook in Google Colab
2. **Install Dependencies**:
   ```
   !pip install streamlit pyngrok transformers torch -q
   ```
3. **Copy Code**: Paste the full code (provided earlier) into a cell
4. **Set ngrok Auth Token**: Replace `YOUR_NGROK_AUTH_TOKEN` with your token
5. **Run the Cell**: Execute the cell to write app.py, set up ngrok, and launch the app
6. **Access the App**: Click the ngrok URL (e.g., https://random-id.ngrok.io) from the output

## Usage Guide

1. **Start the Chatbot**: Open the ngrok URL in your browser
2. **Provide Information**: Respond to prompts for:
   - Full Name
   - Email Address (e.g., example@domain.com)
   - Phone Number (e.g., 123-456-7890)
   - Years of Experience (e.g., 3)
   - Desired Position(s)
   - Current Location
   - Tech Stack (e.g., Python, Flask)
3. **Answer Technical Questions**: Respond to each question one at a time
   - If your answer is unclear (e.g., "idk"), the chatbot will ask for clarification
4. **End the Conversation**: Type "goodbye" or "exit" to conclude
   - You'll receive a farewell message with next steps

## Technical Details

### Libraries Used
- **streamlit**: Frontend interface for the chatbot
- **pyngrok**: Exposes the local server to a public URL
- **transformers & torch**: Power the GPT-2 model for question generation
- **re**: Regular expressions for input validation

### Model Details
- Uses **gpt2** (124M parameters) from Hugging Face's Transformers library for lightweight, free text generation
- Configured with `max_length=300`, `temperature=0.8`, and `top_k=50` for balanced question quality

### Architectural Decisions
- **State Management**: Streamlit's `session_state` tracks conversation context (messages, candidate info, question index)
- **Modular Design**: Separate functions for question generation (`generate_technical_questions`) and input validation (`is_valid_email`, etc.)
- **Sequential Flow**: Questions are asked one at a time, enhancing user experience and context handling

## Prompt Design

### Information Gathering
- Prompts are clear and specific (e.g., "Please provide your email address"), with validation to ensure usable data
- Example: "Nice to meet you, {name}! Please provide your email address" leverages context for personalization

### Technical Question Generation
- **Prompt**:
  ```
  Generate a list of 3 to 5 technical interview questions for a candidate proficient in {tech_stack}. Focus on practical knowledge and problem-solving skills related to these technologies. Format the output as a numbered list (e.g., 1. Question text). Provide only the questions, no extra text.
  ```
- **Why**: Specifies the task (question generation), context (tech stack proficiency), and output format (numbered list), guiding the LLM to produce relevant, structured responses
- **Fallback**: Generic questions are provided if the LLM fails, ensuring the purpose is met

## Challenges & Solutions

- **Challenge**: gpt2 sometimes generated incomplete or irrelevant questions
  - **Solution**: Added a fallback mechanism with tech-stack-specific questions and tuned generation parameters (max_length, temperature)

- **Challenge**: Unexpected user inputs (e.g., "idk") during the questions phase
  - **Solution**: Implemented a check for minimal or unclear answers, prompting for clarification to maintain assessment focus

- **Challenge**: Ending the conversation cleanly without repeated prompts
  - **Solution**: Used `st.session_state.step = "end"` with `return` and `st.rerun()` to halt interaction after "goodbye"

- **Challenge**: Running in Colab with ngrok
  - **Solution**: Wrote the full app to app.py and used pyngrok to expose the server, ensuring compatibility
