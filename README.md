<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Study Timer</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      text-align: center;
      margin: 50px;
      color: white;
      background-color: #001f3f; /* Dark blue background color */
    }

    h1 {
      font-size: 2.5em;
      margin-bottom: 20px;
    }

    #timer {
      font-size: 3em;
      margin-bottom: 20px;
      color: black; /* Black text color for the timer */
    }

    #progress-bar {
      width: 300px;
      height: 20px;
      margin: 0 auto;
      overflow: hidden;
      border-radius: 10px;
      background-color: #ddd; /* Light gray for the progress bar container */
    }

    #progress {
      height: 100%;
      border-radius: 10px;
      transition: width 0.5s;
      background-color: black; /* Black for the progress bar */
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
      color: white;
      background-color: black; /* Black for button background color */
    }

    #clearTasksBtn {
      cursor: pointer;
      background-color: black; /* Black for the clear tasks button */
      color: white; /* White text color for the clear tasks button */
    }

    button:hover {
      background-color: #555; /* Darker gray for button hover effect */
    }

    button:disabled {
      background-color: #95a5a6; /* Gray for disabled button */
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
      cursor: pointer;
      background-color: black; /* Black for the add task button */
    }

    #add-task-btn:hover {
      background-color: #555; /* Darker gray for button hover effect */
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
      background-color: white; /* White for the color dropdown */
      color: black; /* Black text color for the color dropdown */
    }

    #time-input {
      padding: 8px;
      border: none;
      border-radius: 5px;
      background-color: #eee; /* Light gray for the timer input background */
      color: #333; /* Dark gray text color for the timer input */
    }
  </style>
</head>
<body>

  <h1>Study Timer</h1>

  <div>
    <h2>Change Timer Duration (minutes)</h2>
    <input type="number" id="time-input" min="1" value="25">
    <button onclick="changeTimerDuration()">Set Timer</button>
  </div>

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
      <option value="#800000">Maroon</option>
      <option value="#0074cc">Blue</option>
      <option value="#34a853">Green</option>
      <option value="#ea4335">Red</option>
      <option value="#673ab7">Purple</option>
      <option value="#ff9800">Orange</option>
      <option value="#e91e63">Pink</option>
      <option value="#2196f3">Royal Blue</option>
      <option value="#ff5722">Deep Orange</option>
    </select>
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
    }
  </script>

</body>
</html>



