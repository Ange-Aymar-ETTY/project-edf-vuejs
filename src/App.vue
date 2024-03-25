<script>
import Navbar from "./components/Navbar.vue";
import BtnFilter from "./components/BtnFilter.vue";
import CardView from "./components/CardView.vue";

let index = 0;

export default {
  components: {
    Navbar,
    BtnFilter,
    CardView,
  },

  data() {
    return {
      listEvent: [],
      listTeams: [
        {
          id: "1",
          color: "#0080ff",
        },
        {
          id: "2",
          color: "#e78721",
        },
        {
          id: "3",
          color: "#47da5f",
        },
      ],
      showForm: false,
      showFormColor: false,
      filterClicked: [],
      id: "",
      nameEvent: "",
      dateEvent: "",
      team: "",
      numberTeam: "",
      color: "",
      error: "",
    };
  },

  methods: {
    validForm() {
      if (this.nameEvent && this.dateEvent && this.team) {
        if (!this.id) {
          this.listEvent.push({
            id: ++index,
            name: this.nameEvent,
            date: this.dateEvent,
            team: this.team,
            show: true,
          });
        } else {
          const i = this.listEvent.findIndex((e) => e.id === this.id);
          this.listEvent[i].name = this.nameEvent;
          this.listEvent[i].date = this.dateEvent;
          this.listEvent[i].team = this.team;
        }
        this.showForm = false;
        this.id = "";
        this.nameEvent = "";
        this.dateEvent = "";
        this.team = "";
      }
    },

    validFormColor() {
      console.log("coleur", this.color);
      console.log("local", localStorage.getItem("team"));

      if (this.numberTeam && this.color) {
        if (this.listTeams.find((t) => t.id == this.numberTeam)) {
          this.error = "Cette équipe existe";
        } else {
          this.listTeams.push({
            id: this.numberTeam.toString(),
            color: this.color,
          });
          localStorage.removeItem("team");
          localStorage.setItem("team", JSON.stringify(this.listTeams));
          this.showForm = false;
          this.showFormColor = false;
          this.numberTeam = "";
          this.color = "";

          console.log("List", this.listTeams);
        }
      }
    },

    editForm(id) {
      if (id) {
        this.showForm = true;
        const event = this.listEvent.find((e) => e.id === id);
        this.id = event.id;
        this.nameEvent = event.name;
        this.dateEvent = event.date;
        this.team = event.team;
      }
    },

    filtredList(team) {
      if (this.filterClicked.includes(team)) {
        const index = this.filterClicked.findIndex((f) => f == team);
        this.filterClicked.splice(index, 1);
      } else {
        this.filterClicked.push(team);
      }
    },

    colorPick() {
      this.showFormColor = true;
      this.showForm = false;
    },

    formEventClick() {
      this.showForm = true;
      this.showFormColor = false;
    },
  },
  computed: {
    filtredListEvent() {
      if (this.filterClicked.length > 0) {
        return this.listEvent.map((e) => {
          e.show = this.filterClicked.includes(e.team) ? true : false;
          return e;
        });
      } else {
        return this.listEvent.map((e) => {
          e.show = true;
          return e;
        });
      }
    },
  },
  mounted() {
    this.color = "#ffffff";
    if (!localStorage.getItem("team")) {
      localStorage.setItem("team", JSON.stringify(this.listTeams));
    }
    this.listTeams = JSON.parse(localStorage.getItem("team"));
  },
};
</script>

<template>
  <Navbar />

  <div class="btn--area">
    <template v-for="team in listTeams">
      <BtnFilter
        :title="team.id"
        :color="team.color"
        @filter="(filter) => filtredList(filter)"
      />
    </template>
  </div>

  <form class="form" v-if="showForm" @submit.prevent="validForm">
    Nom de l'evenement
    <input type="text" v-model="nameEvent" required />
    Equipe
    <select v-model="team" required>
      <option v-for="(team, key) in listTeams" :key="key" :value="team.id">
        Equipe {{ team.id }}
      </option>
    </select>
    Date de l'evenement
    <input type="date" v-model="dateEvent" required />

    <button class="ea-btn btn--valid" type="submit">Valider</button>
  </form>

  <form class="form" v-if="showFormColor" @submit.prevent="validFormColor">
    Numéro de l'équipe
    <input type="number" min="0" v-model="numberTeam" required />
    <input type="color" v-model="color" required />

    <p v-if="error" style="color: red">{{ error }}</p>

    <button class="ea-btn btn--valid" type="submit">Valider</button>
  </form>

  <span class="btn--back" v-if="showForm || showFormColor" @click="showForm = showFormColor = false"> <- Retour </span>

  <div v-if="showForm == false && showFormColor == false">
    <div class="area">
      <div class="area-button">
        <button class="ea-btn btn--add" @click="formEventClick">Ajouter</button>
        <button class="ea-btn btn--add" @click="colorPick">
          Ajouter une Equipe
        </button>
      </div>

      <div class="list--event">
        <template v-for="(event, key) in filtredListEvent" :key="key">
          <CardView
            v-if="event.show"
            :id="event.id"
            :dateEvent="event.date"
            :nameEvent="event.name"
            :team="event.team"
            @editEvent="(Id) => editForm(Id)"
          />
        </template>
      </div>
    </div>
  </div>
</template>

<style scoped>
.btn--area {
  padding-top: 2rem;
  display: flex;
  gap: 1rem;
}
.area {
  margin-top: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 2rem;
}

.area .list--event {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.area .area-button {
  display: flex;
  gap: 2rem;
}

.form {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;

  width: 20rem;
}

.btn--back {
  margin-left: 2rem;
  color: rgb(49, 49, 243);
  cursor: pointer;
}

.btn--back:hover {
  text-decoration: underline;
}
</style>
