<template>
  <div class="row">
    <div v-if="task" class="col s6 offset-s3">
      <h2>{{ task.title }}</h2>

      <form @submit.prevent="updateTask">
        <div class="input-field">
          <div class="chips" ref="chips"></div>

          <div class="input-field">
            <textarea
              id="description"
              class="materialize-textarea"
              v-model="description"
            ></textarea>
            <label for="description">Description</label>
            <span
              class="character-counter"
              style="float: right; font-size: 12px"
              >{{ description.length }}/2048</span
            >
            <input type="text" ref="datepicker">

            <div v-if="task.status !== 'completed'">
              <button class="btn" type="submit">Update task</button>
              <button
                class="btn blue"
                type="button"
                style="margin-left: 1rem"
                @click="completeTask"
              >
                Complete task
              </button>
            </div>

          </div>
        </div>
      </form>
    </div>

    <div v-else>
      <p>
        Task not found.
        <router-link to="/list">
          <a href="#">Return to the list</a>
        </router-link>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    task() {
      return this.$store.getters.taskById(+this.$route.params.id);
    },
  },
  data: () => ({
    description: "",
    chips: null,
    date: null,
  }),
  mounted() {
    this.description = this.task.description
    this.chips = window.M.Chips.init(this.$refs.chips, {
      placeholder: "Task tags",
      data: this.task.tags,
    });
    this.date = window.M.Datepicker.init(this.$refs.datepicker, {
      format: "dd.mm.yyyy",
      defaultDate: new Date(this.task.date),
      setDefaultDate: true,
    });
    setTimeout(() => {
      window.M.updateTextFields();
    }, 0);
  },
  methods: {
    updateTask() {
      this.$store.dispatch("updateTask", {
        id: this.task.id,
        description: this.description,
        date: this.date.date,
      });
      this.$router.push("/list")
    },
    destroyed(){
      if (this.date && this.date.destroy) {
        this.date.destroy()
      }
      if (this.chips && this.chips.destroy) {
        this.chips.destroy()
      }
    },
    completeTask() {
      this.$store.dispatch("completeTask", this.task.id);
      this.$router.push("/list")
    }
  }
};
</script>
