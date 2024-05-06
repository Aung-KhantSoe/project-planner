<template>
  <div class="home">
    <h1>Home</h1>
    <FilterNav @filterValue="filterValue" :current="current"></FilterNav>
    <div v-for="project in filterProjects" :key="project.id">
      <SingleProject :project="project" @delete="deleteProject" @complete="completeProject"></SingleProject>
    </div>
  </div>
</template>

<script>

import FilterNav from '../components/FilterNav'
import SingleProject from '../components/SingleProject'
export default {
  name: 'HomeView',
  components: {
    FilterNav,
    SingleProject,
    
  },
  data(){
    return {
      projects : [],
      current : 'all'
    }
  },
  methods:{
    deleteProject(id){
      this.projects = this.projects.filter((project)=>{
        return project.id != id;
      });
    },
    completeProject(id){     
      this.projects.find((project)=>{
          if (project.id === id) {
            project.isComplete = !project.isComplete;
          }
        });
      },
      filterValue(value){
        this.current = value;
      }
  },
  computed:{
    filterProjects(){
      switch (this.current) {
        case 'all':
          return this.projects;
        case 'complete':
          return this.projects.filter((project)=>{
              return project.isComplete;
          });
        case 'ongoing':
        return this.projects.filter((project)=>{
              return !project.isComplete;
          });
      
        default:
          return this.projects;
      }
    }
  },

  mounted(){
    fetch('http://localhost:3000/projects')
    .then((response)=>{
      return response.json();
    })
    .then((datas)=>{
      this.projects = datas;
    })
    .catch((err)=>{
      console.log(err);
    })
  }
}
</script>
