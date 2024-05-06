<template>
  <div class="project" :class="{ complete: project.isComplete }">
    <div class="flexing">
      <div @click="toggleDetail">
        <h3>{{ project.title }}</h3>
      </div>
      <div>
        <span class="material-symbols-outlined" @click="deleteProject">
          delete
        </span>
        <router-link :to="{name:'EditProject',params:{id:project.id}}">
          <span class="material-symbols-outlined">
            edit 
          </span>
        </router-link>
        <span class="material-symbols-outlined" @click="completeProject">
          done
        </span>
      </div>
    </div>
    <div v-if="showDetail">
      <p>{{ project.detail }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetail: false,
      api: "http://localhost:3000/projects",
    };
  },
  methods: {
    toggleDetail() {
      this.showDetail = !this.showDetail;
    },
    deleteProject() {
      fetch(this.api + "/" + this.project.id, { method: "DELETE" })
        .then(() => {
          this.$emit("delete", this.project.id);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    completeProject() {
      fetch(this.api + "/" + this.project.id, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          isComplete: !this.project.isComplete,
        }),
      })
        .then(() => {
          this.$emit("complete", this.project.id);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style>
.project {
  padding: 20px;
  background-color: #f2f2f2;
  margin: 10px;
  border-left: 6px solid crimson;
  border-radius: 8px;
}
h3 {
  color: indigo;
  cursor: pointer;
}
.flexing {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
span {
  margin-left: 10px;
  cursor: pointer;
}
span:hover {
  color: #777;
}
.complete {
  border-left: 6px solid teal;
}
</style>