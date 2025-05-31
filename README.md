## Prototype Development for Image Generation Using the Stable Diffusion Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image generation utilizing the Stable Diffusion model, integrated with the Gradio UI framework for interactive user engagement and evaluation.

### PROBLEM STATEMENT:
Users want to leverage the power of advanced AI image generation models like Stable Diffusion, but often lack the technical expertise or desire to interact directly with complex APIs or code. There's a need for a simple, accessible interface that allows non-technical users to input text prompts and receive corresponding images, facilitating creative exploration and model evaluation.

### DESIGN STEPS:

#### STEP 1:Core Model Interaction
Establish a reliable connection to the Stable Diffusion model's API endpoint.

#### STEP 2: Image Data Transformation
Convert the image data received from the API into a format suitable for display in the user interface.

#### STEP 3:User Interface Implementation 
Create an intuitive and interactive web interface for users

### PROGRAM:
```
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']

import requests, json
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_TTI_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }   
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))

import gradio as gr 

#A helper function to convert the PIL image to base64
#so you can send it to the API
def base64_to_pil(img_base64):
    base64_decoded = base64.b64decode(img_base64)
    byte_stream = io.BytesIO(base64_decoded)
    pil_image = Image.open(byte_stream)
    return pil_image

def generate(prompt):
    output = get_completion(prompt)
    result_image = base64_to_pil(output)
    return result_image

gr.close_all()
demo = gr.Interface(fn=generate,
                    inputs=[gr.Textbox(label="Your prompt")],
                    outputs=[gr.Image(label="Result")],
                    title="Image Generation with Stable Diffusion",
                    description="Generate any image with Stable Diffusion",
                    allow_flagging="never",
                    examples=["the spirit of a tamagotchi wandering in the city of Vienna","a mecha robot in a favela"])

demo.launch(share=True, server_port=int(os.environ['PORT1']))
```

### OUTPUT:
![image](https://github.com/user-attachments/assets/229cadec-809a-43b3-a7c9-171ac7c0b6f5)

### RESULT:
Essentially, the code successfully prototypes an "Text-to-Image" generator, making the advanced Stable Diffusion model accessible through a simple web UI, achieving the AIM of interactive user engagement and evaluation.
