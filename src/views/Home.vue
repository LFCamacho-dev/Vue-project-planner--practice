<template>
  <div class="home">

    <FilterNav @filterChange="current = $event" :current="current"/>

    <div v-if="filteredProjects.length">
      
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>     

    </div>




  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'



export default {
  name: 'Home',
  data() {
    return {
      projects: [],
      current: 'all',
      // filteredProjects: []
    }
  },
  components: {
    SingleProject,
    FilterNav,
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => {
      this.projects = data
      // console.log(this.projects);
    })
    .catch(err => console.log(err.message))
  },
  methods: {
    handleDelete(id){
      this.projects = this.projects.filter((project) =>  {
        return project.id !== id
      })
    },
    handleComplete(id){
      let p = this.projects.find(project => project.id === id)
      p.complete = !p.complete
    },    
  },
  computed: {
    filteredProjects(){
      // console.log(this.projects);
      if (this.current === 'completed') {
        return this.projects.filter(project => project.complete === true)
      } else if (this.current === 'ongoing'){
        return this.projects.filter(project => project.complete === false)
      } 
      return this.projects
      
      // console.log(this.projects);

}
  }
}
</script>
