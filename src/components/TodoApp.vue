<template>
  <div class="container">
    <h2 class="text-center mt-5">My Todo App</h2>
    <!--input-->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        class="form-control"
        placeholder="Enter Task Here"
      />
      <button @click="submitTask" class="btn btn-success rounded-0">
        <span class="fa fa-plus"></span>
      </button>
    </div>

    <!--Task Table-->

    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col">#</th>
          <th scope="col">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished: task.status === 'finished' }">{{
              task.name
            }}</span>
          </td>
          <td style="width: 120px">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-warning': task.status === 'in-progress',
                'text-success': task.status === 'finished',
              }"
              >{{ firstCharUpper(task.status) }}</span
            >
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatus: ["to-do", "in-progress", "finished"],

      tasks: [
        {
          name: "steal banana from the store.",
          status: "to-do",
        },
        {
          name: "fuck the hottest girl on campus.",
          status: "in-progress",
        },
        {
          name: "play with my kids.",
          status: "to-do",
        },
      ],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.editTask].name = this.task;
        this.editTask = null;
      }
      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>
<style>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>

