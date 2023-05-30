<script>
  import AppHeader from './components/AppHeader.vue';
  import ProjectCard from './components/ProjectCard.vue';
  import axios from 'axios';

  export default {
    components: { AppHeader , ProjectCard},

    data() {
      return {
        projects: [],
        baseUrl: 'http://localhost:8000',
        currentPage: 1,
        lastPage: null,
      }
    },
    methods: {
      getProjects(gotoPage) {
        // console.log('Ciao mondo!');

        axios.get(`${this.baseUrl}/api/projects`,
          {
            params: {
              page: gotoPage
            }
          }
        )
        .then(response => {
          console.log(response);
          this.projects = response.data.results.data;
          this.currentPage = response.data.results.current_page;
          this.lastPage= response.data.results.last_page;
        })
      },
    },
    mounted() {
      this.getProjects(1);
    }


  };

</script>

<template>

  <AppHeader></AppHeader>
  <ProjectCard :cards="projects"></ProjectCard>

  <nav aria-label="Page navigation example">
    <ul class="pagination ms-5">
      <li class="page-item"><button class="page-link" @click="getProjects(currentPage - 1)" :class="{'disabled' : currentPage == 1}">Previous</button></li>
      <li class="page-item"><button class="page-link" @click="getProjects(currentPage + 1)" :class="{'disabled' : currentPage == lastPage}">Next</button></li>
    </ul>
  </nav>
   
</template>

<style lang="scss">
  @use './styles/general.scss';

</style>
