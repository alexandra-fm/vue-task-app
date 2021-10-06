<template>
  <div class="row">
    <div class="col s6 offset-s3">
      <h2>Create task</h2>

      <form @submit.prevent="submitNewTask">
        <div class="input-field">
          <input
            id="title"
            v-model="title"
            type="text"
            class="validate"
            required
          />
          <label for="first_name">Title</label>

          <span class="helper-text" data-error="Title is required"></span>

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

            <button class="btn" type="submit">Create task</button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "Create",
  data: () => ({
    description: "",
    title: "",
    chips: null,
    date: null,
  }),
  mounted() {
    this.chips = window.M.Chips.init(this.$refs.chips, { placeholder: "Task tags" });
    this.date = window.M.Datepicker.init(this.$refs.datepicker, {
      format: "dd.mm.yyyy",
      defaultDate: new Date(),
      setDefaultDate: true,
    });
  },
  methods: {
    submitNewTask() {
      const task = {
        title: this.title,
        description: this.description,
        id: Date.now(),
        status: "active",
        tags: this.chips.chipsData,
        date: this.date.date,
      }
      this.$store.dispatch("createTask", task);
      this.$router.push("/list")
    },
    destroyed(){
      if (this.date && this.date.destroy) {
        this.date.destroy()
      }
      if (this.chips && this.chips.destroy) {
        this.chips.destroy()
      }
    }
  }
};
</script>
