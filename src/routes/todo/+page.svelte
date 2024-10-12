<script>
    let title = "";
    let description = "";
    let tasks = [
    ];

    async function addTask(title, description) {
        const response = await fetch("http://127.0.0.1:8000/task", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({ title: title, description: description }),
          })
          if (response.ok) {
          title = ""
          description = ""
          getTasks()
          }
    }

    async function getTasks() {
        const response = await fetch("http://127.0.0.1:8000/tasks");
        const data = await response.json();
        console.log(data);
        tasks = data;
    }

    function toggleTask(id) {
      tasks = tasks.map(task =>
        task.id === id ? { ...task, completed: !task.completed } : task
      );
    }

    async function removeTask(id) {
        await fetch(`http://127.0.0.1:8000/task/${id}`, {
            method: "DELETE",
          });
          getTasks()
    }
  </script>

  <style>
    .completed {
      text-decoration: line-through;
    }
    button {
      margin-left: 10px;
    }
  </style>

  <h1>Todo List</h1>

  <input
    type="text"
    placeholder="Title"
    bind:value={title}
    on:keydown={(e) => e.key === "Enter" && addTask()}
  />
<input
    type="text"
    placeholder="Description"
    bind:value={description}
    on:keydown={(e) => e.key === "Enter" && addTask()}
  />
  <button on:click={() => addTask(title, description)}>Add Task</button>

  <ul>
    {#each tasks as task}
      <li>
        <input
          type="checkbox"
          checked={task.completed}
          on:change={() => toggleTask(task.id)}
        />
        <span class:completed={task.completed}>{task.description}</span>
        <button on:click={() => removeTask(task.id)}>Delete</button>
      </li>
    {/each}
  </ul>
