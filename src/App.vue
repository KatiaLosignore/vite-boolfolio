<script>
  import AppHeader from './components/AppHeader.vue';
  import ProjectCard from './components/ProjectCard.vue';
  import axios from 'axios';

  export default {
    components: { AppHeader , ProjectCard},

    data() {
      return {
        projects: [],
        contentMaxLength: 200,
        baseUrl: 'http://localhost:8000',

      }
    },
    methods: {
      getProjects() {
        // console.log('Ciao mondo!');

        axios.get(`${this.baseUrl}/api/projects`)
        .then(response => {
          // console.log(response);
         this.projects = response.data.results;
        })
      },
    },
    mounted() {
      this.getProjects();
    }


  };

</script>

<template>
  
  <AppHeader></AppHeader>
  <ProjectCard :cards="projects"></ProjectCard>
   
</template>

<style lang="scss">
  @use './styles/general.scss';

</style>
