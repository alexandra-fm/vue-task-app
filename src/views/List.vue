<template>
  <div>
    <h2>List</h2>

    <div class="row" >
      <div class="input-field col s3">
        <select ref="select" v-model="filter">
          <option value="" disabled selected>Choose your option</option>
          <option value="active">Active</option>
          <option value="completed">Completed</option>
          <option value="outdated">Outdated</option>
        </select>
        <label>Status filter</label>
      </div>
    </div>

    <button v-if="filter" class="btn btn-small red" @click="filter = null">Clear filter</button>

    <hr />

    <table v-if="tasks.length">
      <thead>
        <tr>
          <th>#</th>
          <th>Title</th>
          <th>Date</th>
          <th>Description</th>
          <th>Status</th>
          <th>Open</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, idx) of displayTasks" :key="task.id">
          <th>{{ idx + 1 }}</th>
          <th>{{ task.title }}</th>
          <th>{{ new Date(task.date).toLocaleDateString() }}</th>
          <th class="th_descr">
            <div class="th_descr_text">{{ task.description }}</div>
          </th>
          <th>{{ task.status }}</th>
          <th>
            <router-link
              tag="button"
              class="btn btn-small"
              :to="'/task/' + task.id"
              >Open</router-link
            >
          </th>
        </tr>
      </tbody>
    </table>
    <router-link v-else to="/">
      <a href="#">No tasks. Create new task!</a>
    </router-link>
  </div>
</template>

<script>
export default {
  data: () => ({
    filter: null 
  }),
  computed: {
    tasks() {
      return this.$store.getters.tasks;
    },
    displayTasks() {
      return this.tasks.filter((task) => {
        if (!this.filter) {
          return true;
        }
        return task.status === this.filter;
      })
    }
  },
  mounted() {
    this.select = window.M.FormSelect.init(this.$refs.select);
  },
};
</script>

<style scoped>
.th_descr {
  max-width: 50px;
}
.th_descr_text{
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
</style>