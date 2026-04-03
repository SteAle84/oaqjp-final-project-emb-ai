"""
This module initiates the Flask application for emotion detection.
It provides routes to render the index page and process emotion
analysis requests using the EmotionDetection package.
"""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

# Initialize the Flask application
app = Flask("Emotion Detector")

@app.route("/emotionDetector")
def sent_analyzer():
    """
    Analyzes the text from the request and returns the emotion scores
    and the dominant emotion as a formatted string.
    """
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)

    # Extracting emotions and dominant emotion from response
    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']

    # Error handling for invalid/blank inputs
    if dominant_emotion is None:
        return "Invalid text! Please try again!"

    # Formatting the output string
    return (
        f"For the given statement, the system response is 'anger': {anger}, "
        f"'disgust': {disgust}, 'fear': {fear}, 'joy': {joy} and "
        f"'sadness': {sadness}. The dominant emotion is {dominant_emotion}."
    )

@app.route("/")
def render_index_page():
    """
    Renders the index.html template for the main user interface.
    """
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
