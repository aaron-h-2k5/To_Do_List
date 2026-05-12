# Ex03 To-Do List using JavaScript
## Date: 12.05.26

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Syne:wght@400..800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="styles.css">
  <title>Basic Todo | Aaron H</title>
</head>
<body>
  <h1>Todos</h1>
  <div class="header">
    <form onsubmit="return false;">
      <input type="text" placeholder="Add your Todos here">
      <button onclick="addTodo()">Add todo</button>
    </form>
  </div>
  <div class="todos"> 
    
  </div>
</body>
<script src="script.js"></script>
</html>


```

```css
:root {
    --bg-color: #1A1A1A;
    --violet: #8338ec;
}

* {
    box-sizing: border-box;
}

body {
    background-color: var(--bg-color);
    color: white;
    font-family: "Syne", sans-serif;
}

h1 {
    text-align: center;
    font-size: 2.5rem;
}

.header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-flow: row wrap;
    margin-bottom: 3rem;
    
}

.header button{
    background-color: var(--violet);
    border: none;
    color: white;
    padding: 1rem;
    border-radius: 8px;
    margin: 1.5rem;
    box-shadow: 5px 5px 10px black;
}

.header button:hover {
    background-color: #6d37b8;
}

.header input {
    flex-basis: 500px;
    padding: 1rem;
    border-radius: 10px;
    border: 1.5px solid var(--violet);
    background: transparent;
    color: white;
}

.header input::placeholder {
    color: white;
}

.header input:focus {
    outline: none;
}

.todos {
    max-width: 768px;
    margin: 0 auto;
}

div[class^="todo-"] {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-flow: row wrap;
    background-color: var(--violet);
    padding: 10px;
    margin-bottom: 1rem;
    color: white;
    border-radius: 10px;
    box-shadow: 5px 10px 15px black;
  }

div[class^="todo-"] .content {
    font-family: "Inter", sans-serif;
    display: flex;
    align-items: center;
}

div[class^="todo-"] .content .todo-checkbox{
    height: 1.5rem;
    width: 1.5rem;
    accent-color: rgb(0, 255, 0);

}

div[class^="todo-"] button {
    margin: 1em;
    padding: 1rem;
    border-radius: 10px;
    font-size: 0.8rem;
    border: none;
    box-shadow: 3px 2px 5px black;
    font-weight: bold;
}

div[class^="todo-"] button:hover {
    opacity: 0.9;
}

div[class^="todo-"] h2 {
    font-size: 1.5rem;
    text-align: left;
    padding-left: 20px;
    border: none;
}



div[class^="todo-"] input {
    font-family: "Inter", sans-serif;
    font-size: 1.2rem;
    font-weight: bold;
    text-wrap: wrap;
    flex: 1;
    padding: 0.7rem 1rem 0.7rem;
    margin-left: 0.7rem;
    border-radius: 25px;
    border: 1px solid white;
    background: transparent;
    color: #ffffff;
    max-width: 100%;
}

div[class^="todo-"] input:focus {
    outline: none;
}

@media (max-width: 400px) {
    .header input {
        flex-basis: 100%;
        margin-bottom: 1rem;
    }

    div[class^="todo-"] input {
        margin: 0.7rem 0; 
    }

    div[class^="todo-"] {
        flex-direction: column;
    }

    div[class^="todo-"] button {
        margin-top: 0.5rem;
    }
}
```

```js
let todos = [];

function loadTodos() {
    const storedTodos = localStorage.getItem("todos");
    if (storedTodos) {
        todos = JSON.parse(storedTodos);
    }
    render();
}

function savelocally() {
    localStorage.setItem("todos", JSON.stringify(todos));
}

function addTodo() {
    const inputEl = document.querySelector(".header input");
    if (!inputEl.value) {
        alert("Todo cannot be empty");
        return;
    }
    todos.push({
        title: inputEl.value,
        isEditing: false,
        isCompleted: false
    });
    inputEl.value = '';
    savelocally()
    render();
}

function deleteTodo(ind) {
    todos.splice(ind, 1);
    savelocally()
    render();
}

function editTodo(ind) {
    todos[ind].isEditing = true;
    render();
}


function saveTodo(ind) {
    const inputEle = document.querySelector(`.todo-${ind} .editinput`);
    if (!inputEle.value){
        alert("Todo cannot be empty");
        return;
    }
    todos[ind].title = inputEle.value;
    todos[ind].isEditing = false;
    savelocally();
    render();
}

function toggleComplete(ind) {
    todos[ind].isCompleted = !todos[ind].isCompleted;
    savelocally();
    render();
}


function todoComponent(todo, ind) {

    const parentEl = document.querySelector(".todos");
    const divEl = document.createElement("div");
    divEl.setAttribute("class", `todo-${ind}`); 

    const todoText = document.createElement("h2");
    todoText.innerHTML = todo.title;
    todoText.classList.add("todo-text");

    const checkbox = document.createElement("input");
    checkbox.type = "radio";
    checkbox.setAttribute("class", "todo-checkbox");
    checkbox.checked = todo.isCompleted;
    checkbox.setAttribute("onclick", `toggleComplete(${ind})`);

    const contentDiv = document.createElement("div");
    contentDiv.setAttribute("class", "content");

    const buttonContainer = document.createElement("div");
    buttonContainer.setAttribute("class", "btnwrapper");

    const deleteButton = document.createElement("button");
    deleteButton.setAttribute("onclick", `deleteTodo(${ind})`);
    deleteButton.innerHTML = "Delete";
    deleteButton.style.backgroundColor = "#F03A47";
    deleteButton.style.color = "#ffffff";

    const editButton = document.createElement("button");
    editButton.setAttribute("onclick", `editTodo(${ind})`);
    editButton.innerHTML = "Edit";
    editButton.style.backgroundColor = "#f5e74e";
    editButton.style.color = "#000000";

    contentDiv.appendChild(checkbox);
    contentDiv.appendChild(todoText);

    buttonContainer.appendChild(editButton);
    buttonContainer.appendChild(deleteButton);

    divEl.appendChild(contentDiv);
    divEl.appendChild(buttonContainer);
    parentEl.appendChild(divEl);


    if (todo.isEditing) {
        const inputEl = document.createElement("input");
        inputEl.setAttribute("class", "editinput")
        inputEl.value = todoText.innerHTML;
        const saveButton = document.createElement("button");
        saveButton.innerHTML = "Save";
        saveButton.setAttribute("onclick", `saveTodo(${ind})`);
        saveButton.setAttribute("class", "savebtn");
        saveButton.style.backgroundColor = "#369f57";
        saveButton.style.color = "#ffffff";
        checkbox.disabled = "true";
        contentDiv.replaceChild(inputEl, todoText);
        buttonContainer.replaceChild(saveButton, editButton);
    }
    if (checkbox.checked) {
        todoText.style.textDecoration = "line-through";
        todoText.style.color = "rgb(0,255,0)";
        editButton.disabled = "true";
        editButton.style.opacity = "0.7";
    }
}

function render() {
    document.querySelector(".todos").innerHTML = '';
    for (let i=0; i<todos.length;i++) {
        todoComponent(todos[i], i);
    }
}

loadTodos();
```

## OUTPUT
<img width="1912" height="1188" alt="Screenshot 2026-05-12 at 2 36 04 PM" src="https://github.com/user-attachments/assets/e0a5dde6-3708-4c5f-b0c9-87f6b9f03188" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
