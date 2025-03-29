## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

Design and implement a system where:

1. Users interact with a chat interface to calculate the volume of a cylinder.
2. User inputs such as the radius and height of the cylinder are extracted via the LLM.
3. The extracted inputs are passed to a Python function that calculates the volume.
4. The LLM integrates this function and provides the result to the user.

### DESIGN STEPS:

#### STEP 1: Define the Python Function
Create a function calculate_cylinder_volume to compute the volume of a cylinder using the formula:
Volume = 𝜋 × 𝑟2 × ℎ
where, r is the radius and ℎ is the height.

#### STEP 2: LLM Integration
Integrate the function with the LLM using a function-calling interface, allowing the LLM to invoke the Python function based on user queries.

#### STEP 3: User Query Parsing
Use the LLM's ability to interpret natural language queries to extract values for radius and height.

#### STEP 4: Function Invocation
Pass the extracted values to the Python function, compute the result, and return it to the LLM.

#### STEP 5: Result Formatting
Present the result in a user-friendly format and provide additional guidance if necessary.

### PROGRAM:

```
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain.output_parsers import StructuredOutputParser, ResponseSchema

# Step 1: Define Parameters and Prompt Template
prompt_template = PromptTemplate(
    input_variables=["radius", "height"],
    template=(
        "You are a helpful math assistant. Extract parameters and calculate cylinder volume.\n"
        "Input:\n"
        "Radius: {radius}\n"
        "Height: {height}\n\n"
        "Provide a JSON response like this:\n"
        "{{\n"
        '  "radius": <value>,\n'
        '  "height": <value>,\n'
        '  "volume": <calculated volume>\n'
        "}}\n\n"
        "Ensure the response includes a valid calculation."
    )
)

# Step 2: Define the Output Parser
response_schemas = [
    ResponseSchema(name="radius", description="The radius of the cylinder."),
    ResponseSchema(name="height", description="The height of the cylinder."),
    ResponseSchema(name="volume", description="The calculated volume of the cylinder."),
]
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)

# Step 3: Create the LangChain LLM Chain with Gemini Model
API_KEY = "**************************"
llm = ChatGoogleGenerativeAI(model="gemini-pro", temperature=0.3, google_api_key=API_KEY)

chain = LLMChain(prompt=prompt_template, llm=llm, output_parser=output_parser)

# Step 4: Execute the Chain with Examples
examples = [
    {"radius": "4", "height": "5"},
    {"radius": "10", "height": "5.7"},  # This should trigger an error or invalid response
]

for example in examples:
    try:
        result = chain.run(example)
        print(f"Input: {example}")
        print(f"Output: {result}\n")
    except Exception as e:
        print(f"Error for input {example}: {e}\n")

```

### OUTPUT:

![Screenshot (179)](https://github.com/user-attachments/assets/412c5d81-8920-49d7-b18f-30c7a072979d)


### RESULT:

Thus, a Python function for calculating the volume of a cylinder integrated with a chat completion system utilizing the function-calling feature of a large language model (LLM) is implemented successfully.
