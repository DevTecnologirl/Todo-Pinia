<template>
  <main>
    <!-- heading -->
    <header>
      <img src="https://pinia.vuejs.org/logo.svg" alt="pinia logo" />
      <h1>Pinia Todo List</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form"><TaskForm /></div>

    <!--loading-->
    <div class="loading" v-if="loading">Loading taks...</div>
    <!--tasks list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>You have {{ favCount }} favs left to do</p>
      <div v-for="task in favs">
        <TaskDetails :task="task" />
      </div>
    </div>
    <!-- filter-->
    <nav class="filter">
      <button @click="filter = 'all'">All Tasks</button>
      <button @click="filter = 'favs'">Favorite Tasks</button>
      <button class="button" @click="taskStore.$reset">Reset</button>
    </nav>
  </main>
</template>

<script>
import { ref } from "vue";
import TaskDetails from "./components/icons/TaskDetails.vue";
import TaskForm from "./components/icons/TaskForm.vue";
import { useTaskStore } from "./stores/TaskStore";
import { storeToRefs } from "pinia";
export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore();
    const { tasks, loading, favs, totalCount, favCount } =
      storeToRefs(taskStore);
    storeToRefs(taskStore);
    //fetch tasks
    taskStore.getTasks();
    const filter = ref("all");
    return {
      taskStore,
      filter,
      tasks,
      loading,
      favs,
      totalCount,
      favCount,
    };
  },
};
</script>
