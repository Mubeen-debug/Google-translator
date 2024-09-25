# Google-translator
from google.cloud import translate_v2 as translate
from flask import Flask, render_template, request

app = Flask(__name__)

# Function to detect and translate the text
def translate_text(text, target_language):
    translate_client = translate.Client()
    
    # Detect the source language and translate
    result = translate_client.translate(text, target_language=target_language)
    
    # Return both the source language and translated text
    return result['detectedSourceLanguage'], result['translatedText']

# Flask route to handle the translation
@app.route('/', methods=['GET', 'POST'])
def index():
    translated_text = ""
    detected_language = ""
    if request.method == 'POST':
        input_text = request.form['input_text']
        target_lang = request.form['target_lang']
        
        # Perform translation
        detected_language, translated_text = translate_text(input_text, target_lang)

    return render_template('index.html', translated_text=translated_text, detected_language=detected_language)

if __name__ == '__main__':
    app.run(debug=True)
