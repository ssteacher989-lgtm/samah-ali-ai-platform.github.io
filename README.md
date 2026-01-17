[index.html](https://github.com/user-attachments/files/24688888/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Samah Ali AI Platform</title>
<script src="https://unpkg.com/tesseract.js@5/dist/tesseract.min.js"></script>
<style>
body {
  font-family: Arial;
  background: linear-gradient(135deg,#667eea,#764ba2);
  padding: 30px;
  user-select: none;
}
.container {
  background: #fff;
  padding: 25px;
  border-radius: 18px;
  max-width: 1000px;
  margin: auto;
  box-shadow: 0 15px 40px rgba(0,0,0,0.25);
}
.brand {
  text-align: center;
  font-size: 26px;
  font-weight: bold;
  color: #4a4a4a;
}
.subtitle {
  text-align: center;
  color: #777;
}
button {
  background: #667eea;
  color: white;
  border: none;
  padding: 10px 15px;
  margin: 5px;
  border-radius: 8px;
  cursor: pointer;
}
textarea {
  width: 100%;
  height: 280px;
  padding: 12px;
  border-radius: 10px;
  border: 1px solid #ccc;
}
footer {
  text-align: center;
  margin-top: 25px;
  color: #eee;
}
</style>
</head>
<body>

<div class="container">
  <div id="brandArea">
    <div class="brand">✨ Samah Ali AI Platform</div>
    <div class="subtitle">Professional Speech & Image to Text Platform</div>
  </div>

  <div style="text-align:center;margin-bottom:10px;" id="welcomeText">
    Welcome to your personal AI writing platform, Samah 🌷
  </div>

  <div>
    <button onclick="startListening()">🎤 Start</button>
    <button onclick="stopListening()">⏹ Stop</button>
    <button onclick="improveText()">✨ Improve Text</button>
    <button onclick="downloadWord()">📄 Download Word</button>
    <button onclick="toggleBrand()">🔖 Branding: <span id="brandStatus">ON</span></button>
  </div>

  <textarea id="output" placeholder="Your professional English text will appear here..."></textarea>

  <br><br>
  <label>📷 Upload Image (OCR):</label>
  <input type="file" accept="image/*" onchange="extractText(this.files[0])">
</div>

<footer id="footerBrand">
  © 2026 Samah Ali – All Rights Reserved
</footer>

<script>
let recognition;
let brandingEnabled = true;
console.log("%cLicensed to Samah Ali - Private Platform", "color:red;font-size:14px");
document.addEventListener("contextmenu", e => e.preventDefault());
document.addEventListener("selectstart", e => e.preventDefault());
setInterval(() => {
  if (window.outerWidth - window.innerWidth > 160 || window.outerHeight - window.innerHeight > 160) {
    document.body.innerHTML = "<h1 style='color:red;text-align:center;margin-top:20%'>Unauthorized Access</h1>";
  }
}, 1200);

function startListening() {
  const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  recognition = new SpeechRecognition();
  recognition.lang = "en-US";
  recognition.continuous = true;
  recognition.onresult = (event) => {
    let text = "";
    for (let i = event.resultIndex; i < event.results.length; i++) {
      text += event.results[i][0].transcript + " ";
    }
    document.getElementById("output").value += text;
    localStorage.setItem("samahText", document.getElementById("output").value);
  };
  recognition.start();
}
function stopListening() { if (recognition) recognition.stop(); }
function improveText() {
  let text = document.getElementById("output").value;
  text = text.replace(/i/g,"I").replace(/dont/gi,"don't").replace(/im/gi,"I'm").replace(/teh/gi,"the").replace(/\s+/g," ").replace(/\. ?/g,". ");
  document.getElementById("output").value = text;
  localStorage.setItem("samahText", text);
}
function toggleBrand() {
  brandingEnabled = !brandingEnabled;
  document.getElementById("brandArea").style.display = brandingEnabled ? "block" : "none";
  document.getElementById("welcomeText").style.display = brandingEnabled ? "block" : "none";
  document.getElementById("footerBrand").style.display = brandingEnabled ? "block" : "none";
  document.getElementById("brandStatus").innerText = brandingEnabled ? "ON" : "OFF";
}
function downloadWord() {
  const text = document.getElementById("output").value;
  let content = brandingEnabled ? `<h2>Samah Ali AI Platform</h2><hr>${text.replace(/
/g,"<br>")}<hr><p style="font-size:12px;color:gray;">© 2026 Samah Ali – All Rights Reserved</p>` : text.replace(/
/g,"<br>");
  const blob = new Blob([`<html><body>${content}</body></html>`], { type: "application/msword" });
  const link = document.createElement("a");
  link.href = URL.createObjectURL(blob);
  link.download = "Document.doc";
  link.click();
}
function extractText(file) {
  Tesseract.recognize(file, 'eng').then(res => {
    document.getElementById("output").value += "
" + res.data.text;
    localStorage.setItem("samahText", document.getElementById("output").value);
  });
}
window.onload = () => {
  const saved = localStorage.getItem("samahText");
  if (saved) document.getElementById("output").value = saved;
};
</script>
</body>
</html>
