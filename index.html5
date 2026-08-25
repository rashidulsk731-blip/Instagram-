<!DOCTYPE html>
<html lang="as">
<head>
<meta charset="UTF-8">

<meta name="viewport"
      content="width=device-width, initial-scale=1.0">

<title>RA STUDIO - Instagram DM Automation</title>

<style>

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #080808;
  color: white;
}

.container {
  max-width: 600px;
  margin: auto;
  padding: 25px;
}

.logo {
  text-align: center;
  font-size: 32px;
  font-weight: bold;
  margin-bottom: 5px;
}

.subtitle {
  text-align: center;
  color: #aaa;
  margin-bottom: 30px;
}

.card {
  background: #151515;
  padding: 22px;
  border-radius: 18px;
  margin-bottom: 20px;
  border: 1px solid #292929;
}

h2 {
  margin-top: 0;
}

label {
  display: block;
  margin: 15px 0 7px;
  color: #ccc;
}

input,
textarea {
  width: 100%;
  padding: 13px;
  border-radius: 10px;
  border: 1px solid #333;
  background: #0d0d0d;
  color: white;
}

textarea {
  min-height: 100px;
  resize: vertical;
}

button {
  width: 100%;
  padding: 14px;
  margin-top: 18px;
  border: 0;
  border-radius: 10px;
  background: white;
  color: black;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
}

button:hover {
  opacity: .85;
}

.result {
  margin-top: 15px;
  padding: 12px;
  border-radius: 10px;
  background: #0d0d0d;
  color: #ddd;
  word-break: break-word;
}

.flow {
  line-height: 2;
  color: #ddd;
}

.badge {
  display: inline-block;
  background: #222;
  padding: 5px 10px;
  border-radius: 20px;
}

</style>
</head>

<body>

<div class="container">

  <div class="logo">
    RA STUDIO
  </div>

  <div class="subtitle">
    Instagram Comment → DM File Delivery
  </div>

  <div class="card">

    <h2>⚙️ Automation Settings</h2>

    <label>Keyword</label>

    <input
      id="keyword"
      value="FILE"
      placeholder="FILE"
    >

    <label>DM Message</label>

    <textarea id="message">ধন্যবাদ! আপুনি বিচৰা File-টো তলৰ Link-ৰ পৰা Download কৰিব পাৰিব:</textarea>

    <button onclick="saveSettings()">
      Save Settings
    </button>

    <div id="settingsResult"
         class="result"></div>

  </div>

  <div class="card">

    <h2>📁 Upload File</h2>

    <input
      type="file"
      id="file"
    >

    <button onclick="uploadFile()">
      Upload File
    </button>

    <div id="uploadResult"
         class="result"></div>

  </div>

  <div class="card">

    <h2>🧪 Test Keyword</h2>

    <p>
      Instagram Comment:
      <span class="badge">FILE</span>
    </p>

    <button onclick="testKeyword()">
      Test Automation
    </button>

    <div id="testResult"
         class="result"></div>

  </div>

  <div class="card">

    <h2>🔄 Flow</h2>

    <div class="flow">

      🎬 Instagram Reel/Post
      <br>
      ↓
      <br>

      💬 Customer comments
      <br>

      <b>FILE</b>
      <br>
      ↓
      <br>

      🤖 Instagram Automation
      <br>
      ↓
      <br>

      📩 Customer DM
      <br>
      ↓
      <br>

      📁 File Download Link

    </div>

  </div>

</div>

<script>

async function saveSettings() {

  const keyword =
    document.getElementById("keyword").value;

  const message =
    document.getElementById("message").value;

  const response = await fetch(
    "/api/settings",
    {
      method: "POST",

      headers: {
        "Content-Type": "application/json"
      },

      body: JSON.stringify({
        keyword,
        message
      })
    }
  );

  const data = await response.json();

  document.getElementById(
    "settingsResult"
  ).innerText =
    data.success
      ? "✅ Settings Save হৈছে।"
      : "❌ Error";
}


async function uploadFile() {

  const file =
    document.getElementById("file").files[0];

  if (!file) {
    alert("File select কৰক।");
    return;
  }

  const formData =
    new FormData();

  formData.append("file", file);

  const response = await fetch(
    "/api/upload",
    {
      method: "POST",
      body: formData
    }
  );

  const data = await response.json();

  document.getElementById(
    "uploadResult"
  ).innerText =
    data.success
      ? `✅ Uploaded!\n${data.url}`
      : `❌ ${data.message}`;
}


async function testKeyword() {

  const response = await fetch(
    "/api/test",
    {
      method: "POST",

      headers: {
        "Content-Type": "application/json"
      },

      body: JSON.stringify({
        comment: "FILE"
      })
    }
  );

  const data = await response.json();

  document.getElementById(
    "testResult"
  ).innerText =
    data.success
      ? `✅ Test successful!\n\n${data.message}`
      : `❌ ${data.message}`;
}

</script>

</body>
</html>
