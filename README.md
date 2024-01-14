<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Study Timer</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 50px;
      color: black; /* Default text color */
    }

    #timer {
      font-size: 2em;
      margin-bottom: 20px;
      outline: 2px solid black; /* Black outline for better visibility */
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
      outline: 2px solid black; /* Black outline for better visibility */
    }

    button:hover {
      background-color: #45a049;
    }

    button:disabled {
      background-color: #aaa;
      cursor: not-allowed;
    }

    #task-list {
      text-align: left;
      margin-top: 20px;
      max-width: 400px;
      margin-left: auto;
      margin-right: auto;
    }

    #task-input {
      width: 70%;
      padding: 8px;
      border: none;
      border-radius: 5px;
      margin-right: 10px;
      outline: 2px solid black; /* Black outline for better visibility */
    }

    #add-task-btn {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 8px 15px;
      border-radius: 5px;
      cursor: pointer;
      outline: 2px solid black; /* Black outline for better visibility */
    }

    #tasks {
      list-style-type: none;
      padding: 0;
      outline: 2px solid black; /* Black outline for better visibility */
    }

    .color-dropdown {
      padding: 8px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      outline: 2px solid black; /* Black outline for better visibility */
    }

    .color-option {
      display: inline-block;
      width: 20px;
      height: 20px;
      margin: 0 5px;
      cursor: pointer;
      outline: 2px solid black; /* Black outline for better visibility */
    }
  </style>
</head>
<body>

  <h1>Study Timer</h1>

  <div id="timer">25:00</div>

  <div id="progress-bar">
    <div id="progress"></div>
  </div>

  <div id="controls">
    <button onclick="startTimer()" id="startButton">Start</button>
    <button onclick="stopTimer()">Stop</button>
  </div>

  <div id="task-list">
    <h2>Task List</h2>
    <input type="text" id="task-input" placeholder="Add a task">
    <button onclick="addTask()" id="add-task-btn">Add</button>
    <ul id="tasks"></ul>
  </div>

  <div>
    <h2>Background Color Options</h2>
    <select id="colorDropdown" class="color-dropdown" onchange="changeBackgroundColor(this.value)">
      <option value="#333">Dark</option>
      <option value="#555">Medium</option>
      <option value="#1e3943">Custom</option>
      <option value="#800000">Red</option>
      <option value="#008000">Green</option>
    </select>
    <div class="color-option" onclick="changeBackgroundColor('#333')"></div>
    <div class="color-option" onclick="changeBackgroundColor('#555')"></div>
    <div class="color-option" onclick="changeBackgroundColor('#1e3943')"></div>
    <div class="color-option" onclick="changeBackgroundColor('#800000')"></div>
    <div class="color-option" onclick="changeBackgroundColor('#008000')"></div>
  </div>

  <script>
    let timer;
    let timeLeft = 1500; // 25 minutes in seconds

    function startTimer() {
      // Disable the start button to prevent multiple clicks
      document.getElementById('startButton').disabled = true;

      timer = setInterval(updateTimer, 1000);
    }

    function stopTimer() {
      clearInterval(timer);
      // Enable the start button when stopping the timer
      document.getElementById('startButton').disabled = false;
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
        // Enable the start button after the timer reaches zero
        document.getElementById('startButton').disabled = false;
      } else {
        timeLeft--;
      }
    }

    function addTask() {
      const taskInput = document.getElementById('task-input');
      const taskText = taskInput.value.trim();

      if (taskText !== '') {
        const tasksList = document.getElementById('tasks');
        const newTask = document.createElement('li');
        newTask.textContent = taskText;
        tasksList.appendChild(newTask);
        taskInput.value = '';
      }
    }

    function changeBackgroundColor(color) {
      document.body.style.backgroundColor = color;
    }
  </script>

</body>
</html>

</body>
</html>

