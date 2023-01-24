<template>
    <div class="kanban-board">
      <div class="section to-do">
        <h3>To Do</h3>
        <div v-for="task in filteredTasks" v-if="task.status === 'to-do'" :key="task.id" class="task" @dragstart="dragTask(task)" @drop="dropTask(task)">
          {{ task.title }}
          <button @click="editTask(task)">Edit</button>
          <button @click="removeTask(task)">Delete</button>
        </div>
      </div>
      <div class="section in-progress">
        <h3>In Progress</h3>
        <div v-for="task in filteredTasks" v-if="task.status === 'in-progress'" :key="task.id" class="task" @dragstart="dragTask(task)" @drop="dropTask(task)">
          {{ task.title }}
          <button @click="editTask(task)">Edit</button>
          <button @click="removeTask(task)">Delete</button>
        </div>
      </div>
      <div class="section done">
        <h3>Done</h3>
        <div v-for="task in filteredTasks" v-if="task.status === 'done'" :key="task.id" class="task" @dragstart="dragTask(task)" @drop="dropTask(task)">
          {{ task.title }}
          <button @click="editTask(task)">Edit</button>
          <button @click="removeTask(task)">Delete</button>
        </div>
      </div>
      <div class="add-task">
        <input type="text" v-model="newTaskTitle" placeholder="Enter task title">
        <select v-model="newTaskStatus">
          <option value="to-do">To Do</option>
          <option value="in-progress">In Progress</option>
          <option value="done">Done</option>
        </select>
        <button @click="addTask">Add Task</button>
      </div>
      <div class="search-task">
        <input type="text" v-model="searchText" placeholder="Search task">
      </div>
    </div>
  </template>

<script>
export default {
  data() {
    return {
      tasks: [
        { id: 1, title: 'Task 1', status: 'to-do' },
        { id: 2, title: 'Task 2', status: 'in-progress' },
        { id: 3, title: 'Task 3', status: 'done' }
      ],
      newTaskTitle: '',
      newTaskStatus: 'to-do',
      searchText: '',
    }
  },
  computed: {
    filteredTasks() {
      return this.tasks.filter(task => task.title.toLowerCase().includes(this.searchText.toLowerCase()))
    }
  },
  methods: {
    dragTask(task) {
      // Set the task being dragged as the current task
      this.currentTask = task;
    },
    dropTask(task) {
      // Update the task's status with the current task's status
      task.status = this.currentTask.status;
      // Make a PUT request to the backend to update the task
      axios.put('/api/tasks/' + task.id, {
        title: task.title,
        status: task.status
      })
      .then(response => {
        // Handle successful response
      })
      .catch(error => {
        // Handle error
      });
    },
    editTask(task) {
      // Make a PUT request to the backend to update the task
      axios.put('/api/tasks/' + task.id, {
        title: task.title,
        status: task.status
      })
      .then(response => {
        // Handle successful response
      })
      .catch(error => {
        // Handle error
      });
    },
    removeTask(task) {
      // Make a DELETE request to the backend to remove the task
      axios.delete('/api/tasks/' + task.id)
      .then(response => {
        // Remove the task from the tasks array in the frontend
        this.tasks = this.tasks.filter(t => t.id !== task.id);
      })
      .catch(error => {
        // Handle error
      });
    },
    addTask() {
      // Make a POST request to the backend to add the new task
      axios.post('/api/tasks', {
        title: this.newTaskTitle,
        status: this.newTaskStatus
      })
      .then(response => {
        // Add the new task to the tasks array in the frontend
        this.tasks.push(response.data);
        // Clear the form inputs
        this.newTaskTitle = '';
        this.newTaskStatus = 'to-do';
      })
      .catch(error => {
        // Handle error
      });
    }
  }
  }
</script>
