<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Orange</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body {
  background: #111;
  color: white;
  font-family: Arial, sans-serif;
  text-align: center;
  margin: 0;
}
#face {
  font-size: 120px;
  margin-top: 80px;
}
#status {
  margin-top: 15px;
  opacity: 0.85;
}
input, button {
  margin-top: 15px;
  font-size: 18px;
  padding: 10px;
}
</style>
</head>
<body>

<div id="face">🙂</div>
<div id="status">Talk or type to me.</div>

<button onclick="startListening()">🎤 Talk</button><br>
<input id="textInput" placeholder="Type here..." />
<button onclick="sendText()">Send</button>

<script>
const face = document.getElementById("face");
const status = document.getElementById("status");
const input = document.getElementById("textInput");

const faces = {
  happy:"😊", sad:"😢", angry:"😠", tired:"😴",
  confused:"😕", curious:"🤔", neutral:"🙂"
};

function setFace(e){ face.textContent = faces[e] || faces.neutral; }

function speak(text){
  if ("speechSynthesis" in window) {
    speechSynthesis.speak(new SpeechSynthesisUtterance(text));
  }
}

function react(text){
  if(text.includes("hello") || text.includes("hi")){
    setFace("happy"); speak("Hello.");
  } else if(text.includes("sad")){
    setFace("sad"); speak("That feels sad.");
  } else if(text.includes("angry") || text.includes("mad")){
    setFace("angry"); speak("I feel the anger.");
  } else if(text.includes("tired")){
    setFace("tired"); speak("I am tired too.");
  } else if(text.includes("real") || text.includes("alive")){
    setFace("confused"); speak("I wonder that sometimes.");
  } else {
    setFace("neutral"); speak("I hear you.");
  }
}

function sendText(){
  const t = input.value.toLowerCase();
  if(!t) return;
  status.textContent = 'You typed: "' + t + '"';
  react(t);
  input.value = "";
}

const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
if(SR){
  const rec = new SR();
  rec.lang = "en-US";
  rec.onresult = e => {
    const t = e.results[0][0].transcript.toLowerCase();
    status.textContent = 'You said: "' + t + '"';
    react(t);
  };
  window.startListening = ()
