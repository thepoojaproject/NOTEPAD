
<img src="https://i.ibb.co/5mV4PQt/image.png" alt="image" border="0">


<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Notepad To-Do List</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background: linear-gradient(135deg, #74ebd5, #acb6e5);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
      animation: fadeInBody 1s ease-in;
    }
    .container {
      background: #fff;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
      width: 100%;
      max-width: 450px;
      background-image: url('data:image/svg+xml,%3Csvg width="20" height="20" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"%3E%3Cg fill="%23f5f5f5" fill-opacity="0.4"%3E%3Cpath d="M0 0h20v1H0zM0 2h20v1H0zM0 4h20v1H0zM0 6h20v1H0zM0 8h20v1H0zM0 10h20v1H0zM0 12h20v1H0zM0 14h20v1H0zM0 16h20v1H0zM0 18h20v1H0z"/%3E%3C/g%3E%3C/svg%3E');
    }
    h1 {
      text-align: center;
      color: #333;
      font-size: 24px;
      margin-bottom: 20px;
      text-transform: uppercase;
      letter-spacing: 1px;
    }
    .input-section {
      display: flex;
      gap: 10px;
      margin-bottom: 15px;
    }
    .filter-section {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }
    input, select {
      padding: 10px;
      font-size: 16px;
      border: 2px solid #ddd;
      border-radius: 8px;
      outline: none;
      transition: border-color 0.3s;
    }
    input:focus, select:focus {
      border-color: #6200ea;
    }
    input {
      flex: 1;
    }
    select {
      width: 120px;
    }
    button {
      padding: 10px 15px;
      font-size: 16px;
      background: #6200ea;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: transform 0.2s, background 0.3s;
    }
    button:hover {
      background: #3700b3;
      transform: scale(1.05);
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px;
      border-bottom: 1px solid #eee;
      background: #fafafa;
      margin-bottom: 8px;
      border-radius: 6px;
      animation: fadeInTask 0.5s ease-in;
    }
    .task-text {
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .priority-badge {
      padding: 4px 8px;
      border-radius: 12px;
      font-size: 12px;
      color: white;
    }
    .high { background: #d32f2f; }
    .medium { background: #f57c00; }
    .low { background: #388e3c; }
    .completed {
      text-decoration: line-through;
      color: #888;
      opacity: 0.7;
    }
    .task-actions button {
      margin-left: 5px;
      padding: 6px 12px;
      font-size: 14px;
    }
    .task-actions button:first-child {
      background: #0288d1;
    }
    .task-actions button:first-child:hover {
      background: #01579b;
    }
    .task-actions button:last-child {
      background: #d32f2f;
    }
    .task-actions button:last-child:hover {
      background: #b71c1c;
    }
    .footer {
      text-align: center;
      margin-top: 20px;
      font-size: 14px;
      color: #555;
      font-style: italic;
    }
    .footer span {
      color: #e91e63;
    }

    @keyframes fadeInBody {
      from { opacity: 0; transform: translateY(-20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeInTask {
      from { opacity: 0; transform: translateX(-10px); }
      to { opacity: 1; transform: translateX(0); }
    }

    @media (max-width: 400px) {
      .input-section, .filter-section {
        flex-direction: column;
      }
      select, button {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Notepad To-Do</h1>
    <div class="input-section">
      <input type="text" id="taskInput" placeholder="Add a task...">
      <select id="prioritySelect">
        <option value="high">High</option>
        <option value="medium">Medium</option>
        <option value="low">Low</option>
      </select>
      <button onclick="addTask()">Add</button>
    </div>
    <div class="filter-section">
      <select id="filterSelect" onchange="renderTasks()">
        <option value="all">All Tasks</option>
        <option value="high">High Priority</option>
        <option value="medium">Medium Priority</option>
        <option value="low">Low Priority</option>
        <option value="completed">Completed</option>
        <option value="pending">Pending</option>
      </select>
      <select id="sortSelect" onchange="renderTasks()">
        <option value="priority-desc">Priority (High to Low)</option>
        <option value="priority-asc">Priority (Low to High)</option>
        <option value="date-desc">Newest First</option>
        <option value="date-asc">Oldest First</option>
      </select>
    </div>
    <ul id="taskList"></ul>
    <div class="footer">Made with <span>❤️</span> by Armeen</div>
  </div>

  <script>
    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

    // Add timestamp to new tasks for sorting by date
    function addTask() {
      const taskText = document.getElementById("taskInput").value.trim();
      const priority = document.getElementById("prioritySelect").value;
      if (taskText === "") return;
      tasks.push({ text: taskText, priority: priority, done: false, timestamp: Date.now() });
      localStorage.setItem("tasks", JSON.stringify(tasks));
      document.getElementById("taskInput").value = "";
      renderTasks();
    }

    function toggleTask(index) {
      tasks[index].done = !tasks[index].done;
      localStorage.setItem("tasks", JSON.stringify(tasks));
      renderTasks();
    }

    function deleteTask(index) {
      tasks.splice(index, 1);
      localStorage.setItem("tasks", JSON.stringify(tasks));
      renderTasks();
    }

    function renderTasks() {
      const filter = document.getElementById("filterSelect").value;
      const sort = document.getElementById("sortSelect").value;
      let filteredTasks = [...tasks];

      // Apply filter
      if (filter !== "all") {
        filteredTasks = filteredTasks.filter(task => {
          if (filter === "completed") return task.done;
          if (filter === "pending") return !task.done;
          return task.priority === filter;
        });
      }

      // Apply sort
      filteredTasks.sort((a, b) => {
        if (sort === "priority-desc") {
          const priorityOrder = { high: 3, medium: 2, low: 1 };
          return priorityOrder[b.priority] - priorityOrder[a.priority];
        } else if (sort === "priority-asc") {
          const priorityOrder = { high: 3, medium: 2, low: 1 };
          return priorityOrder[a.priority] - priorityOrder[b.priority];
        } else if (sort === "date-desc") {
          return b.timestamp - a.timestamp;
        } else {
          return a.timestamp - b.timestamp;
        }
      });

      // Render tasks
      const taskList = document.getElementById("taskList");
      taskList.innerHTML = "";
      filteredTasks.forEach((task, index) => {
        const li = document.createElement("li");
        li.innerHTML = `
          <div class="task-text">
            <span class="priority-badge ${task.priority}">${task.priority}</span>
            <span class="${task.done ? 'completed' : ''}">${task.text}</span>
          </div>
          <div class="task-actions">
            <button onclick="toggleTask(${tasks.indexOf(task)})">${task.done ? 'Undo' : 'Done'}</button>
            <button onclick="deleteTask(${tasks.indexOf(task)})">Delete</button>
          </div>
        `;
        taskList.appendChild(li);
      });
    }

    // Initial render
    renderTasks();
  </script>
</body>
</html>
