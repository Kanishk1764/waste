---

# **TalentScout Hiring Assistant**  
**Project Documentation**  
**By Kanishk Mishra**  
**Email: Kanishkmishra402@gmail.com**  

---

## **Introduction**  
The **TalentScout Hiring Assistant** is an AI-powered chatbot designed to streamline the initial screening process for tech recruitment. Built using **Streamlit** for the front-end interface and **Groq's LLM API** for natural language processing, this tool automates the collection of candidate information, assesses technical proficiency, and provides a seamless conversational experience.  

The chatbot is capable of:  
1. Collecting essential candidate details (name, email, phone, experience, etc.).  
2. Generating technical questions based on the candidate's tech stack.  
3. Maintaining context-aware conversations with sentiment analysis and multilingual support.  
4. Storing candidate data in a simulated database for further review.  

---

## **Key Features**  

### **1. Smart Information Collection**  
The chatbot dynamically collects the following candidate information:  
- **Full Name**  
- **Email Address**  
- **Phone Number**  
- **Years of Experience**  
- **Desired Position**  
- **Current Location**  
- **Tech Stack**  

Each piece of information is requested in a conversational manner, ensuring a natural and engaging interaction.  

### **2. Technical Question Generation**  
Based on the candidate's declared tech stack (e.g., Python, Django, React), the chatbot generates **3-5 technical questions** to assess their proficiency. These questions are tailored to the candidate's skills and are designed to evaluate both theoretical knowledge and practical application.  

### **3. Sentiment Analysis**  
Using **TextBlob**, the chatbot analyzes the sentiment of the candidate's responses. This allows the chatbot to adjust its tone—becoming more empathetic for negative sentiments or more enthusiastic for positive ones.  

### **4. Multilingual Support**  
The chatbot supports multiple languages using **langdetect** for language detection and **GoogleTranslator** for translation. This ensures that candidates can interact in their preferred language.  

### **5. Simulated Database**  
All candidate data is stored in a simulated in-memory database using **Streamlit's session state**. This data can be viewed and exported by administrators for further review.  

---

## **Technical Implementation**  

### **1. Backend**  
The backend is powered by **Groq's LLM API**, which handles all conversational logic and response generation. The chatbot uses a state machine to manage the conversation flow, ensuring that all required information is collected systematically.  

#### **Code Example: State Management**  
```python
def process_input(self, user_input, current_state, candidate_info, asked_questions):
    # Detect language and translate input to English
    src_lang = self.detect_language(user_input)
    user_input_en = self.translate_text(user_input, src_lang, "en") if src_lang != "en" else user_input
    
    # Analyze sentiment
    sentiment_score = self.analyze_sentiment(user_input_en)
    
    # Generate a response based on the current state
    messages = [
        {"role": "system", "content": f"""
        You are a hiring assistant for TalentScout. The current state of the conversation is: {current_state}. 
        The candidate has provided the following information so far: {candidate_info}. 
        Respond appropriately based on the user input: {user_input_en}.
        
        The sentiment analysis of the candidate's input is: {"positive" if sentiment_score > 0 else "negative" if sentiment_score < 0 else "neutral"}.
        Adjust your tone accordingly to be empathetic and supportive if the sentiment is negative, or enthusiastic if the sentiment is positive.
        """},
        {"role": "user", "content": user_input_en}
    ]
    response_en = self.model.generate_response(messages)
    
    # Translate response back to the candidate's language
    response = self.translate_text(response_en, "en", src_lang) if src_lang != "en" else response_en
    
    # Handle conversation flow
    if current_state == "greeting":
        return self.collect_name(user_input, candidate_info, response)
    elif current_state == "collect_name":
        return self.collect_email(user_input, candidate_info, response)
    # ... (other states)
```

### **2. Frontend**  
The frontend is built using **Streamlit**, a Python library for creating web applications. The interface is simple and intuitive, with a chat-based layout that mimics real-world messaging apps.  

#### **Code Example: Streamlit Interface**  
```python
def main():
    st.set_page_config(page_title="TalentScout Hiring Assistant", page_icon="🤖", layout="wide")
    st.title("TalentScout Hiring Assistant")
    st.markdown("#### Tech Recruitment Initial Screening")
    
    # Initialize session state for chat history
    if "messages" not in st.session_state:
        st.session_state.messages = []
        st.session_state.candidate_info = {
            "name": None,
            "email": None,
            "phone": None,
            "experience": None,
            "position": None,
            "location": None,
            "tech_stack": None
        }
        st.session_state.current_state = "greeting"
        st.session_state.asked_questions = []
    
    # Display chat history
    for message in st.session_state.messages:
        with st.chat_message(message["role"]):
            st.markdown(message["content"])
    
    # Get user input
    user_input = st.chat_input("Type your response here...")
    
    if user_input:
        # Process user input and update chat history
        response = assistant.process_input(user_input, st.session_state.current_state, st.session_state.candidate_info, st.session_state.asked_questions)
        st.session_state.messages.append({"role": "assistant", "content": response["message"]})
        with st.chat_message("assistant"):
            st.markdown(response["message"])
```

---

## **Conclusion**  
The **TalentScout Hiring Assistant** is a powerful tool for automating the initial stages of tech recruitment. By leveraging AI and natural language processing, it provides a seamless and efficient experience for both candidates and recruiters.  

This project demonstrates the potential of AI in transforming traditional hiring processes, making them faster, smarter, and more accessible.  

---

## **How to Use**  
1. Clone the repository.  
2. Install dependencies:  
   ```bash
   pip install streamlit groq textblob langdetect deep-translator pandas
   ```  
3. Run the application:  
   ```bash
   streamlit run app.py
   ```  
4. Interact with the chatbot via the Streamlit interface.  

---

## **Contact**  
For any inquiries or feedback, feel free to reach out:  
**Email:** Kanishkmishra402@gmail.com  

---

