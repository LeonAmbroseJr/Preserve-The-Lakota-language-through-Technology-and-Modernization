# Preserve-The-Lakota-language-through-Technology-and-Modernization
My attempt to create an app, and/or website that will help preserve the Lakota Language,  attempting to make minor adjustments and more appealing to the youth . Modernize not the language but how it is presented to the future generations. 


"""
Grandmother-friendly Lakota pronunciation practice
Run:  python app.py
Then open http://localhost:5000 in any browser.
"""
from flask import Flask, render_template, request, send_from_directory
import os, datetime, uuid

app = Flask(__name__)
AUDIO_DIR = "recordings"
os.makedirs(AUDIO_DIR, exist_ok=True)

@app.route("/")
def home():
    return render_template("index.html")

@app.route("/record", methods=["POST"])
def record():
    """Save the learner's audio blob as .webm, then convert to .wav."""
    blob = request.files["audio"]
    filename = f"{datetime.date.today()}_{uuid.uuid4().hex}.wav"
    path = os.path.join(AUDIO_DIR, filename)
    blob.save(path)
    return {"status": "ok", "file": filename}

@app.route("/audio/<path:name>")
def static_audio(name):
    return send_from_directory("static/audio", name)

if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=5000)


<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Speak Lakota</title>
  <style>
    body   { background:#111;color:#fff;text-align:center;font-family:sans-serif }
    button { font-size:4rem;padding:2rem 4rem;border-radius:1rem;border:none;
             background:#f0c020;cursor:pointer }
    button:hover { background:#ffd54f }
  </style>
</head>
<body>
  <h1>Press to Speak Lakota</h1>
  <button id="recBtn">🦬 Press & Hold</button>
  <audio id="model" src="/audio/elder_sample.wav" preload="auto"></audio>
  <script>
    const btn = document.getElementById('recBtn');
    let mediaRecorder, chunks=[];

    // Ask browser for mic permission once
    navigator.mediaDevices.getUserMedia({audio:true})
      .then(stream => { mediaRecorder = new MediaRecorder(stream); })
      .catch(err => alert("Please allow microphone access."));

    // When user presses button: play elder voice + start recording
    btn.onmousedown = () => {
      document.getElementById('model').play();
      chunks=[];
      mediaRecorder.ondataavailable = e => chunks.push(e.data);
      mediaRecorder.start();
    };

    // When user releases button: stop recording, send to server
    btn.onmouseup = () => {
      mediaRecorder.stop();
      mediaRecorder.onstop = () => {
        const blob = new Blob(chunks, {type:"audio/webm"});
        const fd   = new FormData();
        fd.append("audio", blob);
        fetch("/record", {method:"POST", body:fd});
        alert("Recording saved for elder review!");
      };
    };
  </script>
</body>
</html>



git clone https://gist.github.com/<your-gist-id>  lakota-demo
cd lakota-demo
python app.py



mkdir lakota-pronounce-demo
cd lakota-pronounce-demo
# copy the 3 files you already have
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/<your-username>/lakota-pronounce-demo.git
git push -u origin main


echo "Flask==2.3.3" > requirements.txt
git add requirements.txt && git commit -m "add requirements" && git push



# Python
__pycache__/
*.pyc
*.pyo
*.pyd
env/
venv/
.venv/

# Flask / local run
instance/
*.log

# Audio recordings that users create locally
recordings/*.wav
recordings/*.webm

# OS junk
.DS_Store
Thumbs.db
desktop.ini

# IDE / editor settings
.vscode/
.idea/
*.swp



## License
- Source code: MIT © 2025 Lakota Language Community  
- Audio, stories, images: CC BY-NC-SA 4.0



    
