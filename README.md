
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
      color: white;
      background-color: #001f3f; /* Default dark blue background color */
    }

    #timer {
      font-size: 2em;
      margin-bottom: 20px;
    }

    #progress-bar {
      width: 300px;
      height: 20px;
      margin: 0 auto;
      overflow: hidden;
      border-radius: 10px;
    }

    #progress {
      height: 100%;
      border-radius: 10px;
      transition: width 0.5s;
    }

    #controls {
      margin-top: 20px;
    }

    button {
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

    #clearTasksBtn {
      background-color: #f44336; /* Red for visibility on dark backgrounds */
      color: white;
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
    }

    #add-task-btn {
      background-color: #4CAF50;
      color: white;
      cursor: pointer;
    }

    #tasks {
      list-style-type: none;
      padding: 0;
    }

    #colorDropdown {
      padding: 8px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    #time-input {
      padding: 8px;
      border: none;
      border-radius: 5px;
    }
  </style>
</head>
<body>

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
    <button onclick="clearTasks()" id="clearTasksBtn">Clear Tasks</button>
    <ul id="tasks"></ul>
  </div>

  <div>
    <h2>Background Color Options</h2>
    <select id="colorDropdown" onchange="changeTheme(this.value)">
      <option value="#001f3f">Dark Blue</option>
      <option value="#555555">Gray</option>
      <option value="#0074cc">Blue</option>
      <option value="#34a853">Green</option>
      <option value="#ea4335">Red</option>
      <option value="#673ab7">Purple</option>
      <option value="#ff9800">Orange</option>
      <option value="#e91e63">Pink</option>
    </select>
  </div>

  <div>
    <h2>Change Timer Duration (minutes)</h2>
    <input type="number" id="time-input" min="1" value="25">
    <button onclick="changeTimerDuration()">Set Timer</button>
  </div>

  <script>
    let timer;
    let timeLeft = 1500; // Default timer duration is 25 minutes

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

    function clearTasks() {
      const tasksList = document.getElementById('tasks');
      tasksList.innerHTML = '';
    }

    function changeTimerDuration() {
      const timeInput = document.getElementById('time-input');
      const newDuration = parseInt(timeInput.value, 10) * 60;
      timeLeft = newDuration;
      stopTimer();
      updateTimer();
    }

    function changeTheme(color) {
      document.body.style.backgroundColor = color;
      document.getElementById('timer').style.color = getContrastColor(color);
      document.getElementById('progress').style.backgroundColor = getContrastColor(color);
      document.getElementById('startButton').style.backgroundColor = getContrastColor(color);
      document.getElementById('add-task-btn').style.backgroundColor = getContrastColor(color);
      document.getElementById('clearTasksBtn').style.backgroundColor = getContrastColor(color);
    }

    function getContrastColor(hexColor) {
      // Function to get contrasting text color based on background color
      const r = parseInt(hexColor.slice(1, 3), 16);
      const g = parseInt(hexColor.slice(3, 5), 16);
      const b = parseInt(hexColor.slice(5, 7), 16);
      const yiq = ((r * 299) + (g * 587) + (b * 114)) / 1000;
      return (yiq >= 128) ? '#000000' : '#ffffff';
    }
  </script>

</body>
</html>

