<template>
  <div class="todo-container">
    <h1>ToDo List</h1>

    <!-- Input untuk menambahkan task baru -->
    <input 
      v-model="newTask" 
      @keyup.enter="isEditing ? updateTask() : addTask()" 
      placeholder="Tambah task baru"
    />
    <button @click="isEditing ? updateTask() : addTask()">
      {{ isEditing ? 'Update' : 'Tambah' }}
    </button>

    <!-- List task -->
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <span :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">
          {{ task.text }}
        </span>
        <button @click="toggleComplete(index)">
          {{ task.completed ? 'Batal' : 'Selesai' }}
        </button>
        <button @click="editTask(index)">Edit</button>
        <button @click="removeTask(index)">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '', // Input untuk task baru
      tasks: [], // List task
      isEditing: false, // Apakah sedang mengedit atau tidak
      currentIndex: null // Index task yang sedang diedit
    };
  },
  methods: {
    // Tambah task baru
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = ''; // Reset input setelah task ditambahkan
      }
    },
    // Toggle status selesai atau belum selesai
    toggleComplete(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
    // Hapus task
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    // Edit task
    editTask(index) {
      this.isEditing = true;
      this.newTask = this.tasks[index].text;
      this.currentIndex = index;
    },
    // Update task setelah di-edit
    updateTask() {
      if (this.newTask.trim() !== '') {
        this.tasks[this.currentIndex].text = this.newTask;
        this.newTask = ''; // Reset input setelah update
        this.isEditing = false;
        this.currentIndex = null;
      }
    }
  }
};
</script>

<style scoped>
.todo-container {
  width: 400px;
  margin: 50px auto;
  text-align: center;
}
button {
  margin-left: 10px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 10px 0;
}
</style>
