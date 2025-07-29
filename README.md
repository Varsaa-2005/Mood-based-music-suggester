# Mood-based-music-suggester
from flask import Flask, render_template_string, request

app = Flask(__name__)

# Sample mood-based music suggestions
music_db = {
    "happy": [
        "Pharrell Williams - Happy",
        "Katrina & The Waves - Walking on Sunshine",
        "Justin Timberlake - Can't Stop the Feeling!"
    ],
    "sad": [
        "Adele - Someone Like You",
        "Sam Smith - Stay With Me",
        "Coldplay - The Scientist"
    ],
    "energetic": [
        "Queen - Don't Stop Me Now",
        "Survivor - Eye of the Tiger",
        "Daft Punk - Harder, Better, Faster, Stronger"
    ],
    "relaxed": [
        "Norah Jones - Come Away With Me",
        "Jack Johnson - Better Together",
        "Jason Mraz - I'm Yours"
    ]
}

HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>Mood Based Music Suggester</title>
    <style>
        body {
            font-family: Arial, sans-serif; 
            background: #f0f0f0; 
            padding: 40px;
            max-width: 600px;
            margin: auto;
            color: #333;
        }
        h1 {
            text-align: center;
            color: #4CAF50;
        }
        form {
            text-align: center;
            margin-bottom: 30px;
        }
        select {
            padding: 10px;
            font-size: 16px;
        }
        button {
            padding: 10px 15px;
            font-size: 16px;
            margin-left: 10px;
            cursor: pointer;
        }
        ul {
            list-style-type: none;
            padding-left: 0;
        }
        li {
            background: white;
            margin: 8px 0;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <h1>Mood Based Music Suggester</h1>
    <form method="POST">
        <label for="mood">Select your mood:</label>
        <select name="mood" id="mood" required>
            <option value="" disabled selected>Choose a mood</option>
            {% for mood in moods %}
            <option value="{{mood}}" {% if selected_mood == mood %}selected{% endif %}>{{mood.title()}}</option>
            {% endfor %}
        </select>
        <button type="submit">Get Songs</button>
    </form>
    {% if songs %}
    <h2>Recommended songs for mood: "{{selected_mood.title()}}"</h2>
    <ul>
        {% for song in songs %}
        <li>{{song}}</li>
        {% endfor %}
    </ul>
    {% endif %}
</body>
</html>
"""

@app.route("/", methods=["GET", "POST"])
def index():
    selected_mood = None
    songs = []
    if request.method == "POST":
        selected_mood = request.form.get("mood")
        songs = music_db.get(selected_mood, [])
    return render_template_string(HTML_TEMPLATE, moods=music_db.keys(), songs=songs, selected_mood=selected_mood)

if __name__ == "__main__":
    app.run(debug=True)
