<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content="AnonMind is a 100% free and anonymous platform where anyone can share their thoughts, feelings, problems, or happiness — through text or voice — without revealing any identity. No login, no phone number, no email. Just speak your heart out. Completely secure and under full control of the creator." />
  <meta name="keywords" content="anonymous thoughts, free voice sharing, no login, share problems, mental health support, speak freely, AnonMind" />
  <meta name="author" content="AnonMind Creator" />
  <title>AnonMind - Share Freely, Stay Anonymous</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #111;
      color: #f5f5f5;
      margin: 0;
      padding: 0;
    }
    header {
      background: #222;
      padding: 1rem;
      text-align: center;
      font-size: 1.5rem;
      font-weight: bold;
      color: #00ffc8;
    }
    main {
      padding: 2rem;
      max-width: 800px;
      margin: auto;
    }
    textarea {
      width: 100%;
      height: 100px;
      padding: 1rem;
      border: none;
      border-radius: 10px;
      margin-bottom: 1rem;
      font-size: 1rem;
    }
    button {
      padding: 0.75rem 1.5rem;
      font-size: 1rem;
      border: none;
      border-radius: 8px;
      background: #00ffc8;
      color: #111;
      cursor: pointer;
    }
    .thought {
      background: #1c1c1c;
      padding: 1rem;
      border-radius: 10px;
      margin-bottom: 1rem;
      white-space: pre-wrap;
    }
    #thoughts {
      margin-top: 2rem;
    }
    #recorder {
      margin-top: 1rem;
    }
  </style>
</head>
<body>
  <header>AnonMind - Share Freely, Stay Anonymous</header>
  <main>
    <h2>Express Your Thoughts</h2>
    <textarea id="thoughtInput" placeholder="Type your thought here in any language..."></textarea>
    <button onclick="postThought()">Share Anonymously</button>

    <div id="recorder">
      <p><strong>Record Your Voice:</strong></p>
      <button onclick="startRecording()">Start Recording</button>
      <button onclick="stopRecording()">Stop</button>
      <div id="audioPreview"></div>
    </div>

    <div id="thoughts">
      <h3>Shared Thoughts:</h3>
      <div id="thoughtList"></div>
    </div>
  </main>

  <script>
    let mediaRecorder;
    let audioChunks = [];

    function postThought() {
      const input = document.getElementById('thoughtInput');
      const text = input.value.trim();
      if (text) {
        const div = document.createElement('div');
        div.className = 'thought';
        div.textContent = text;
        document.getElementById('thoughtList').prepend(div);
        input.value = '';
      }
    }

    async function startRecording() {
      const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
      mediaRecorder = new MediaRecorder(stream);

      mediaRecorder.ondataavailable = event => {
        audioChunks.push(event.data);
      };

      mediaRecorder.onstop = () => {
        const blob = new Blob(audioChunks, { type: 'audio/webm' });
        const audioURL = URL.createObjectURL(blob);
        const audio = document.createElement('audio');
        audio.controls = true;
        audio.src = audioURL;
        document.getElementById('audioPreview').appendChild(audio);
        audioChunks = [];
      };

      mediaRecorder.start();
    }

    function stopRecording() {
      if (mediaRecorder) {
        mediaRecorder.stop();
      }
    }
  </script>
</body>
</html>
