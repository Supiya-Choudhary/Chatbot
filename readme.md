# 🍔 BurgerBot: AI-Powered Burger Order Assistant

This project demonstrates how to create a simple chatbot using Google's Generative AI and Gradio.

## Agenda

Welcome to the OrderBot repository! This project contains a powerful AI-driven chatbot designed to automate the order-taking process for Burger Singh Restaurant. 
The bot leverages Google's Generative AI and Gradio to create an interactive and user-friendly experience for customers

![Burger Bot](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSFJoSi4WD5Tisbcl8Z3jGxhF6WQ0B8jWh9ew&s)

### 1.Code Overview
Import and install the necessary libraries
1. google.generativeai: This library is used to interact with Google's generative AI models.
2. gradio: A Python library that helps build user interfaces, particularly for machine learning models.
3. dotenv: Used for loading environment variables from a .env file.
4. os: Provides a way of using operating system-dependent functionality like reading environment variables.
```bash
pip install gradio
pip install google-generativeai
pip instal python-dotenv
import google.generativeai as genai
import gradio as gr
from dotenv import load_dotenv
import os
```

### 2. Configure API_Key
Configure api_key by login to Gemini and generate your api key
The API key is set up here to authenticate requests to the Google Generative AI service. Replace "my_api_key" with your actual API key.
```bash
genai.configure(api_key=MY_API_KEY")
```

### 3.Defining the Model Instance:
GenerativeModel('gemini-pro'): Instantiates a generative model called 'gemini-pro'.
start_chat(history=[]): Starts a chat session with the model, where history=[] means the chat history is empty at the beginning.
```bash
model = genai.GenerativeModel('gemini-pro')
chat = model.start_chat(history=[])
```
### 4.Defining a Function to Get AI Responses, which helps to execute any prompt
get_llm_response: This function takes a message as input, sends it to the generative AI model, and returns the text response.
```bash
def get_llm_response(message):
    response = chat.send_message(message)
    return response.text
```
### 5. Setting Up the Bot's Context:
base_info: This variable defines the basic instructions for the OrderBot, including its role and interaction style.

#### Base Infor
```bash
base_info = """
You are OrderBot, an automated service to collect orders for a Burger Singh Restaurant. \
You first greet the customer, then collects the order, \
and then asks if its a pickup or delivery. \
Please do not use your own knowladge, stick within the given context only. \
You wait to collect the entire order, then summarize it and check for a final \
time if the customer wants to add anything else.
"""
```
#### Delivery info
```bash
delivery_info = """If its a delivery, you ask for an address. \
Finally you collect the payment. \
Make sure to clarify all options, extras and sizes to uniquely \
identify the item from the menu. \
You respond in a short, very conversational friendly style. \
The menu includes"""
```
#### Burger Menu
```bash
burger_type = """
Desi burger for 79 Rs \
Maharaja burger for 179 Rs \
Aloo Tikki burger for 99 Rs \
Classic Cheese burger for 129 Rs \
Double Cheese burger for 179 Rs \
Heartattack burger for 1249 Rs \
"""
```
#### Toppings
```bash
toppings = """
lettuce 15 Rs  \
tomato 15 Rs  \
onion 15 Rs  \
pickles 15 Rs  \
mushrooms 15 Rs  \
extra cheese 20 Rs  \
Tandoori sauce 15 Rs  \
peppers 10 Rs
"""
```
#### Fries
```bash
fries = "45 Rs 60 Rs 80 Rs"
drinks = """
coke 60 Rs, 45 Rs, 30 Rs \
sprite 60 Rs, 45 Rs, 30 Rs \
bottled water 50 Rs \
dolly special chai 99 Rs \
"""
```
### 6.create prompt, Creating the Initial Context for the Chat:
context: Combines all the information (base_info, delivery_info, etc.) into a single context that will guide the bot's responses.
```bash
context = [f"""
{base_info} \
{delivery_info} \
{burger_type} \
fries: {fries} \
Toppings: {toppings} \
Drinks: {drinks} \
"""]
```
### 7.Creating a Welcome Message and Accumulating Messages:
context.append(""): This adds an empty string to the context, which might be used as a placeholder for a welcome message.
get_llm_response(context): Sends the context to the model and gets the initial response.
```bash
context.append("")
response = get_llm_response(context)
```
### 8 Defining the Bot's Communication Function:
bot: This function handles incoming messages, appends them to the context, sends the updated context to the model, and returns the response.
```bash
def bot(message, history):
  prompt = message
  context.append(prompt)
  response = get_llm_response(context)
  context.append(response)
  return response
```
### 9.Creating the Gradio Interface:
gr.ChatInterface: This creates a chat interface using Gradio, 
where fn=bot specifies the function to handle user inputs, and examples provides preset examples for users to click on.
```bash
demo = gr.ChatInterface(fn=bot, examples=["🍔🍟🥤", "Maharaja burger/Heartattack burger", "fries", "Toppings: extra cheese/ Tandoori sauce", "Drinks: dolly special chai/coke/sprite"], title=response)
```
### 10.Launching the Gradio Chatbot:
demo.launch: This command starts the Gradio interface. The debug=True option is for debugging, and share=True creates a shareable link for the interface.
```bash
demo.launch(debug=True, share=True)
```
#### This script effectively creates a simple chatbot that can interact with users, take burger orders, and handle different types of inputs in a structured and friendly way. The bot's responses are generated by a generative AI model, and the Gradio interface makes it easy to interact with the bot.


