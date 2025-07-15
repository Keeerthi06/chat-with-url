# chat-with-url

import requests
from bs4 import BeautifulSoup
import google.generativeai as genai
import datetime

# Step 1: Load content from Wikipedia
url = "https://en.wikipedia.org/wiki/Artificial_intelligence"
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")
text = soup.get_text()

# Step 2: Configure Gemini
genai.configure(api_key="enter the api here")
model = genai.GenerativeModel("models/gemini-1.5-flash-latest")

# Step 3: Initialize chat history
chat_history = []

# Step 4: Define your question(s)
question = "Based on this content, what are the different types of AI?"

# Step 5: Generate answer using Gemini
full_prompt = f"{question}\n\n{text[:16000]}"  # Ensure it stays within token limit
response = model.generate_content(full_prompt)
answer = response.text.strip()

# Step 6: Save to history
chat_history.append({
    "timestamp": datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
    "question": question,
    "answer": answer
})

# Step 7: Display the response
print("✅ Gemini Answer:\n")
print(answer)

# Step 8: Display full chat history
print("\n🕘 Chat History:\n")
for i, entry in enumerate(chat_history):
    print(f"🗓 {entry['timestamp']}")
    print(f"❓ Question {i+1}: {entry['question']}")
    print(f"💬 Answer:\n{entry['answer'][:500]}{'...' if len(entry['answer']) > 500 else ''}")
    print("-" * 80)
