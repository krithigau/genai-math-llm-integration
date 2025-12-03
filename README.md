## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the area of sphere, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
Design and implement a Python program that calculates the surface area of a sphere by integrating a mathematical function with a chat completion system using the function-calling feature of a large language model (LLM). The program should extract the sphere’s radius from the LLM’s response, perform the calculation, and display the result.

### DESIGN STEPS:

#### STEP 1:
Import the necessary libraries and create a function to calculate the surface area of a sphere.

#### STEP 2:
Define the function schema, create message prompts, and call the LLM using openai.ChatCompletion.create() with the model, messages, and functions.

#### STEP 3:
Extract the radius from the LLM response, convert it to a float, and pass it to the area_of_sphere() function to calculate and display the result.

### PROGRAM:
~~~
# Name: KRITHIGA U
# Regno: 212223240076
import math
import os
from dotenv import load_dotenv, find_dotenv
import openai

# Load environment variables
_ = load_dotenv(find_dotenv())  
openai.api_key = os.environ['OPENAI_API_KEY']

# Step 1: Function to calculate area of a sphere
def area_of_sphere(radius): 
    if radius <= 0:
        return "Enter a valid value."
    area = 4 * math.pi * (radius ** 2)
    return round(area, 2)

# Step 2: Chat with OpenAI and define function schema
def chat_with_openai(prompt):
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are an assistant that helps calculate the area of a sphere."},
            {"role": "user", "content": prompt},
        ],
        functions=[
            {
                "name": "area_of_sphere",
                "description": "Calculate the surface area of a sphere given radius.",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "radius": {"type": "number", "description": "Radius of the sphere (in units)"}
                    },
                    "required": ["radius"],
                },
            }
        ],
        function_call="auto",  
    )
    
    if "function_call" in response["choices"][0]["message"]:
        function_name = response["choices"][0]["message"]["function_call"]["name"]
        arguments = eval(response["choices"][0]["message"]["function_call"]["arguments"])
        if function_name == "area_of_sphere":
            radius = float(arguments["radius"])
            return area_of_sphere(radius)
    
    return response["choices"][0]["message"]["content"]

# Step 3: Take input and display result
radius = float(input("Enter the radius of the sphere: "))
prompt = f"What is the surface area of a sphere with a radius of {radius}?"
result = chat_with_openai(prompt)
print("Result:", result)

print("\n=== Code Execution Successful ===")

~~~

### OUTPUT:
<img width="843" height="170" alt="Screenshot 2025-09-19 101805" src="https://github.com/user-attachments/assets/7268d77b-cfa1-4d6e-8462-97425b251c08" />

### RESULT:
Thus we have designed and implemented a Python function for calculating the area of sphere, integrated it with a chat completion system utilizing the function-calling feature of a large language model (LLM).
