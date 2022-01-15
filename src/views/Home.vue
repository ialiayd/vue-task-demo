<template>
  <!--conditional ifadeler için v-if kullanılabilir. Display için condition belirtirsek v-show da diyebiliriz-->
  <!-- <div v-if="showAddTask">
    <AddTask @add-task="addTask" />
  </div> -->
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder"
    :tasks="tasks"
  />
  <!-- emit added and drilled to task.vue -->
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },

  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, { method: 'DELETE' });
        if (res.ok) this.tasks = this.tasks.filter((x) => x.id !== id);
        else alert('Had error deleting');
      }
    },
    async toggleReminder(id) {
      const taskToToggle = this.tasks.find((x) => x.id === id);
      taskToToggle.reminder = !taskToToggle.reminder;
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(taskToToggle),
      });

      if (res.ok) {
        const data = await res.json();
        this.tasks = this.tasks.map((t) =>
          t.id === id ? { ...t, reminder: data.reminder } : t
        );
      }
    },
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();
      if (data) this.tasks.push(data);
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const json = await res.json();
      return json;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const json = await res.json();
      return json;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<style></style>
