# Intro-project

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">

  <title>Simple To-Do List</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>To-Do List</h1>
    <div class="input-container">
      <input type="text" id="task-input" placeholder="Enter a task" />
      <button id="add-btn">Add Task</button>
    </div>
    <ul id="task-list"></ul>
  </div>
  <script src="script.js"></script>
</body>
</html>

////css

body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f3f4f6;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.container {
  width: 90%;
  max-width: 400px;
  background: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

.input-container {
  display: flex;
  gap: 10px;
}

#task-input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

#add-btn {
  padding: 10px 20px;
  border: none;
  background: #4caf50;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

#add-btn:hover {
  background: #45a049;
}

ul {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}

li {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  background: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-bottom: 10px;
}

li .edit, li .delete {
  margin-left: 10px;
  cursor: pointer;
  color: #007bff;
}

li .delete {
  color: #e63946;
}
//javascript

// Select elements
const taskInput = document.getElementById("task-input");
const addBtn = document.getElementById("add-btn");
const taskList = document.getElementById("task-list");

// Load tasks from LocalStorage
let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

// Render tasks
function renderTasks() {
  taskList.innerHTML = ""; // Clear the list
  tasks.forEach((task, index) => {
    const li = document.createElement("li");
    li.innerHTML = `
      <span>${task}</span>
      <div>
        <button class="edit" onclick="editTask(${index})">Edit</button>
        <button class="delete" onclick="deleteTask(${index})">Delete</button>
      </div>
    `;
    taskList.appendChild(li);
  });
}

// Add task
addBtn.addEventListener("click", () => {
  const task = taskInput.value.trim();
  if (task) {
    tasks.push(task);
    saveTasks();
    renderTasks();
    taskInput.value = ""; // Clear input
  } else {
    alert("Please enter a task!");
  }
});

// Edit task
function editTask(index) {
  const newTask = prompt("Edit your task:", tasks[index]);
  if (newTask !== null && newTask.trim() !== "") {
    tasks[index] = newTask.trim();
    saveTasks();
    renderTasks();
  }
}

// Delete task
function deleteTask(index) {
  tasks.splice(index, 1);
  saveTasks();
  renderTasks();
}

// Save tasks to LocalStorage
function saveTasks() {
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

// Initial render
renderTasks();
