# Blog-Generator

This code is a Streamlit application that uses the LLaMA 2 model to generate blog content based on user inputs. The application takes a blog topic, word count, and target audience style from the user and generates a blog using the specified LLaMA 2 model.

### Imports

import streamlit as st
from langchain.prompts import PromptTemplate
from langchain.llms import CTransformers


- `streamlit`: A framework for building web applications in Python.
- `PromptTemplate` and `CTransformers` from `langchain`: Used for defining the prompt and interacting with the LLaMA 2 model.

### Function to Get Response from LLaMA 2 Model


def getLLamaresponse(input_text, no_words, blog_style):
    llm = CTransformers(model='models/llama-2-7b-chat.ggmlv3.q8_0.bin',
                        model_type='llama',
                        config={'max_new_tokens': 256,
                                'temperature': 0.01})
    
    template = """
        Write a blog for {blog_style} job profile for a topic {input_text}
        within {no_words} words.
            """
    
    prompt = PromptTemplate(input_variables=["blog_style", "input_text", "no_words"],
                            template=template)
    
    response = llm(prompt.format(blog_style=blog_style, input_text=input_text, no_words=no_words))
    print(response)
    return response


- `getLLamaresponse`: This function generates a response from the LLaMA 2 model.
  - `llm`: Initializes the LLaMA 2 model with the specified configuration.
  - `template`: Defines the prompt template for generating the blog content.
  - `prompt`: Formats the template with the given inputs (blog style, topic, and word count).
  - `response`: Generates the response from the model based on the formatted prompt.
  - The response is printed and returned.

### Streamlit Application


st.set_page_config(page_title="Generate Blogs",
                    page_icon='🤖',
                    layout='centered',
                    initial_sidebar_state='collapsed')

st.header("Generate Blogs 🤖")

input_text = st.text_input("Enter the Blog Topic")

col1, col2 = st.columns([5, 5])

with col1:
    no_words = st.text_input('No of Words')
with col2:
    blog_style = st.selectbox('Writing the blog for',
                              ('Researchers', 'Data Scientist', 'Common People'), index=0)
    
submit = st.button("Generate")

if submit:
    st.write(getLLamaresponse(input_text, no_words, blog_style))


- `st.set_page_config`: Configures the Streamlit app with a title, icon, layout, and sidebar state.
- `st.header`: Sets the header of the app.
- `input_text`: Text input for the blog topic.
- `col1, col2`: Creates two columns for additional input fields.
  - `no_words`: Text input for the number of words.
  - `blog_style`: Dropdown to select the target audience style (Researchers, Data Scientists, or Common People).
- `submit`: Button to generate the blog.
- If the button is clicked, the `getLLamaresponse` function is called with the user inputs, and the generated blog is displayed.

This Streamlit application allows users to easily generate blog content tailored to specific audiences using the LLaMA 2 model.
