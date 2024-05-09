```
from pathlib import Path
import hashlib
import google.generativeai as genai

genai.configure(api_key="SUA_CHAVE_API")

# Set up the model
generation_config = {
  "temperature": 0.9,
  "top_p": 0.95,
  "top_k": 32,
  "max_output_tokens": 1024,
}

safety_settings = [
  {
    "category": "HARM_CATEGORY_HARASSMENT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_HATE_SPEECH",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
]

model = genai.GenerativeModel(model_name="gemini-1.0-pro-vision-latest",
                              generation_config=generation_config,
                              safety_settings=safety_settings)

prompt_parts = [
  "Given an image of a product and its target audience, write an engaging marketing description",
  "Product Image: ",
  genai.upload_file("/content/cerejas.png"),
  "Target Audience: Teachers",
  "Marketing Description: This is a fruit - a delicious fruit; it's a symbol of your passion for life and love for your self health.",
  "Product Image: ",
  genai.upload_file("/content/frutas.png"),
  "Target Audience: Nutritionists",
  "Marketing Description: Looking for a sustainable and eco-friendly way to life? Look no further than this fruits. Eat a couple of fruits every day!",
  "Product Image: ",
  genai.upload_file("/content/meme.png"),
  "Target Audience: Comedians",
  "Marketing Description: ",
]

response = model.generate_content(prompt_parts)
print(response.text)
```
