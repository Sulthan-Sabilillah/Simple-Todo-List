<script>
export default {
  data() {
    return {
      Task: '',
      TaskList: [],
      LOCAL_STORAGE_KEY: 'AppKeys'
    }
  },

  mounted() {
    const data = localStorage.getItem(this.LOCAL_STORAGE_KEY)
    if (data) {
      this.TaskList = JSON.parse(data)
    }
  },

  methods: {
    createDataFunc() {
      if (this.Task.trim() !== '') {
        // check if Task is not empty
        const id = new Date().getTime().toString() // Menggunakan waktu saat ini sebagai kunci "id"
        const newTask = {
          id: id,
          name: this.Task,
          isComplete: false
        }
        this.TaskList.push(newTask)
        localStorage.setItem(this.LOCAL_STORAGE_KEY, JSON.stringify(this.TaskList))
        this.Task = '' // Clear the input field
      }
    },

    clearDataFunc() {
      alert('Hapus Semua Data')
      this.TaskList = [] // Clear TaskList Array
      localStorage.setItem(this.LOCAL_STORAGE_KEY, JSON.stringify(this.TaskList))
    },

    toggleComplete(task) {
      const foundTask = this.TaskList.find((item) => item.id === task.id)
      if (foundTask) {
        foundTask.isComplete = !foundTask.isComplete
        localStorage.setItem(this.LOCAL_STORAGE_KEY, JSON.stringify(this.TaskList))
      }
    },

    deleteTaskById(task) {
      const index = this.TaskList.findIndex((item) => item.id === task.id)
      if (index !== -1) {
        const data = this.TaskList.splice(index, 1)
        localStorage.setItem(this.LOCAL_STORAGE_KEY, JSON.stringify(data))
      }
    }
  }
}
</script>

<template>
  <main>
    <div
      class="container d-flex flex-column justify-center rounded-lg outline outline-offset-4 outline-pink-400 mx-auto mt-12 sm:mt-20 md:mt-24"
    >
      <div class="card-header rounded-lg bg-gradient-to-r from-[#D4145A] to-[#FBB03B] py-2 mb-10">
        <h1 class="text-center text-3xl sm:text-5xl font-normal">Todo Apps</h1>
      </div>
      <div class="card-body px-5 pt-5 flex flex-col md:flex-row">
        <input
          v-model="Task"
          class="px-4 py-1.5 mx-auto md:mx-4 rounded-sm text-black"
          type="text"
          placeholder="Masukkan Data"
        />
        <div class="flex justify-center mt-4 md:mx-0 mx-auto">
          <button
            @click="createDataFunc"
            class="px-4 py-1.5 rounded-sm bg-gradient-to-r from-blue-600 to-cyan-600"
          >
            Add
          </button>
          <button
            @click="clearDataFunc"
            class="px-4 py-1.5 ml-2 rounded-sm bg-gradient-to-r from-red-600 to-pink-600"
          >
            Clear
          </button>
        </div>
      </div>
      <div class="card-body grid grid-cols-1 md:grid-cols-2 gap-4 px-5 py-5 mx-4 mt-5">
        <div class="task-ongoing">
          <h3 class="text-lg sm:text-2xl font-normal">Task Left</h3>
          <hr />
          <div>
            <template v-for="(Task, index) in TaskList" :key="index">
              <div
                v-if="!Task.isComplete"
                class="p-4 my-4 bg-gradient-to-r from-[#662D8C] to-[#ED1E79] rounded flex justify-between items-center"
              >
                <p class="break-all truncate max-w-[100px] md:max-w-[110px]">{{ Task.name }}</p>
                <div class="button">
                  <button @click="deleteTaskById(Task)" class="bg-black rounded p-2">
                    <i class="bi bi-x"></i>
                  </button>
                  <button @click="toggleComplete(Task)" class="bg-black rounded p-2 ml-1">
                    <i class="bi bi-check"></i>
                  </button>
                </div>
              </div>
            </template>
          </div>
        </div>
        <div class="task-clear">
          <h3 class="text-lg sm:text-2xl font-normal">Task Clear</h3>
          <hr />
          <div>
            <template v-for="(Task, index) in TaskList" :key="index">
              <div
                v-if="Task.isComplete"
                class="p-4 my-4 bg-gradient-to-r from-[#662D8C] to-[#ED1E79] rounded flex justify-between items-center"
              >
                <p class="break-all truncate max-w-[100px] md:max-w-[110px]">{{ Task.name }}</p>
                <div class="button">
                  <button @click="deleteTaskById(Task)" class="bg-black rounded p-2">
                    <i class="bi bi-x"></i>
                  </button>
                  <button @click="toggleComplete(Task)" class="bg-black rounded p-2 ml-1">
                    <i class="bi bi-arrow-counterclockwise"></i>
                  </button>
                </div>
              </div>
            </template>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>