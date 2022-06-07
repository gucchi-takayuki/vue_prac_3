<template>
  <div id="app">
    <!-- タスク一覧 -->
    <section id="task-list">
      <h1>ToDoリスト</h1>
      <div>
        <input
          id="all"
          name="status"
          type="radio"
          value="all"
          v-model="selectedStatus"
        />
        <label for="all">すべて</label>

        <input
          id="doing"
          name="status"
          value="doing"
          type="radio"
          v-model="selectedStatus"
        />
        <label for="doing">作業中</label>

        <input
          id="done"
          name="status"
          value="done"
          type="radio"
          v-model="selectedStatus"
        />
        <label for="done">完了</label>
      </div>

      <div>
        <table>
          <tr>
            <th>ID</th>
            <th>コメント</th>
            <th>状態</th>
          </tr>
          <tr v-for="(task, index) in filterTasks" :key="index">
            <td>{{ task.id }}</td>
            <td>{{ task.comment }}</td>
            <td>
              <button @click="changeStatus(index)">{{ task.status }}</button>
            </td>
            <td><button @click="deleteTask(index)">削除</button></td>
          </tr>
        </table>
      </div>
    </section>
    <!-- 新規タスク追加 -->
    <section id="add-new-task">
      <h2>新規タスクの追加</h2>
      <input type="text" v-model="newTask" />
      <button @click="addNewTask">追加</button>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedStatus: "all",
      tasks: [],
      newTask: "",
    };
  },
  methods: {
    addNewTask() {
      this.tasks.push({
        id: this.tasks.length + 1,
        comment: this.newTask,
        status: "作業中",
      });
      this.newTask = "";
    },
    renumberId() {
      this.tasks.forEach((task, index) => {
        task.id = index + 1;
      });
    },
    deleteTask(taskIndex) {
      this.tasks.splice(taskIndex, 1);
      this.renumberId();
    },
    changeStatus(taskIndex) {
      const targetTask = this.tasks[taskIndex];
      if (targetTask.status === "作業中") {
        targetTask.status = "完了";
      } else if (targetTask.status === "完了") {
        targetTask.status = "作業中";
      }
    },
  },
  computed: {
    filterTasks() {
      if (this.selectedStatus === "doing") {
        return this.tasks.filter(task => task.status === "作業中");
      } else if (this.selectedStatus === "done") {
        return this.tasks.filter(task => task.status === "完了");
      } else {
        return this.tasks;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
#add-new-task {
  button {
    margin-left: 5px;
  }
}

.inactive {
  display: none;
}
</style>
