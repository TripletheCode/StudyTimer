
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 50px;
      background-color: #333;
      color: white;
    }

    #timer {
      font-size: 2em;
      margin-bottom: 20px;
    }

    #progress-bar {
      width: 300px;
      height: 20px;
      background-color: #555;
      margin: 0 auto;
      overflow: hidden;
      border-radius: 10px;
    }

    #progress {
      height: 100%;
      background-color: #4CAF50;
      border-radius: 10px;
      transition: width 0.5s;
    }

    #controls {
      margin-top: 20px;
    }

    button {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 10px 20px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      margin: 4px 2px;
      cursor: pointer;
      border-radius: 5px;
    }

    button:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>

  <div id="timer">25:00</div>

  <div id="progress-bar">
    <div id="progress"></div>
  </div>

  <div id="controls">
    <button onclick="startTimer()">Start</button>
    <button onclick="stopTimer()">Stop</button>
  </div>

  <script>
    let timer;
    let timeLeft = 1500; // 25 minutes in seconds

    function startTimer() {
      timer = setInterval(updateTimer, 1000);
    }

    function stopTimer() {
      clearInterval(timer);
    }

    function updateTimer() {
      const minutes = Math.floor(timeLeft / 60);
      const seconds = timeLeft % 60;

      document.getElementById('timer').innerText = `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;

      // Update progress bar
      const progress = ((1500 - timeLeft) / 1500) * 100;
      document.getElementById('progress').style.width = `${progress}%`;

      if (timeLeft === 0) {
        clearInterval(timer);
        alert('Time is up! Take a break.');
      } else {
        timeLeft--;
      }
    }
  </script>

</body>
</html>
