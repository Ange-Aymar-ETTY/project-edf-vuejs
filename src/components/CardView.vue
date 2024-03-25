<script>
export default {
  emits: ["editEvent"],
  props: {
    id: { type: Number, required: true },
    dateEvent: { type: String, required: true },
    nameEvent: { type: String, required: true },
    team: { type: String, required: true },
  },
  methods: {
    editEvent() {
      this.$emit("editEvent", this.id);
    }
  },
};
</script>

<template>
  <div class="card-container">
    <div class="part">
      <span>
        <b>Equipe {{ team }}</b> <br />
        {{ dateEvent }}
      </span>

      <button @click="editEvent">Modifier</button>
    </div>

    <div class="event">
      <span>
        <b>{{ nameEvent }}</b>
      </span>
    </div>
  </div>
</template>

<style scoped>
.card-container {
  width: 25rem;
  height: 5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border: 1px solid #ddd;
  border-radius: 7px;
  padding: 0.5rem;
}

.card-container .part {
  display: flex;
  justify-content: space-between;
}

.card-container .event {
  text-align: center;
}
</style>
