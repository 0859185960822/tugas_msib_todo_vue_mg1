<template>
  <div class="container mt-5">
    <div class="card shadow">
      <div class="card-body">
        <h1 class="card-title text-center">ToDo List</h1>
        
        <!-- Input untuk menambahkan task baru -->
        <div class="input-group mb-3">
          <input 
            type="text" 
            class="form-control" 
            placeholder="Tambah task baru" 
            v-model="newTask" 
            @keyup.enter="isEditing ? updateTask() : addTask()" 
          />
          <button 
            class="btn btn-primary" 
            @click="isEditing ? updateTask() : addTask()"
          >
            {{ isEditing ? 'Update' : 'Tambah' }}
          </button>
        </div>

        <!-- List task -->
        <ul class="list-group">
          <li 
            v-for="(task, index) in tasks" 
            :key="index" 
            class="list-group-item d-flex justify-content-between align-items-center"
          >
            <span :class="{ 'text-decoration-line-through': task.completed }">
              {{ task.text }}
            </span>
            <div>
              <button class="btn btn-success btn-sm me-2" @click="toggleComplete(index)">
                {{ task.completed ? 'Batal' : 'Selesai' }}
              </button>
              <button class="btn btn-warning btn-sm me-2" @click="editTask(index)">Edit</button>
              <button class="btn btn-danger btn-sm" @click="removeTask(index)">Hapus</button>
            </div>
          </li>
        </ul>
      </div>
    </div>
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
  mounted() {
    // Memuat data dari localStorage saat aplikasi dimuat
    const savedTasks = localStorage.getItem('tasks');
    if (savedTasks) {
      this.tasks = JSON.parse(savedTasks);
    }
  },
  methods: {
    // Tambah task baru
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = ''; // Reset input setelah task ditambahkan
        this.saveToLocalStorage(); // Simpan ke localStorage
      }
    },
    // Toggle status selesai atau belum selesai
    toggleComplete(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
      this.saveToLocalStorage(); // Simpan ke localStorage
    },
    // Hapus task
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveToLocalStorage(); // Simpan ke localStorage
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
        this.saveToLocalStorage(); // Simpan ke localStorage
      }
    },
    // Simpan data ke localStorage
    saveToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
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
