<script>
  import ProjectCard from '../components/ProjectCard.vue';
  import axios from 'axios';
  import { store } from '../store.js';

  export default {
    name: 'ProjectList',
    
    data() {
        return {
            projects: [],
            store,
            currentPage: 1,
            lastPage: null,
            loading: true
        }
    },
    components: {
         ProjectCard 
    },
    methods: {
      getProjects(gotoPage) {
        this.loading = true;

        axios.get(`${this.store.baseUrl}/api/projects`,
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
          this.loading = false;
        })
      },
    },
    mounted() {
      this.getProjects(1);
    }


  };

</script>

<template>
   <div v-if="loading == false">
    <ProjectCard :cards="projects"></ProjectCard>
  </div>
  <div v-else>
    <img src="/loader.gif" alt="caricamento in corso..." />
  </div>

  <nav aria-label="Page navigation example">
    <ul class="pagination ms-5">
      <li class="page-item"><button class="page-link" @click="getProjects(currentPage - 1)" :class="{'disabled' : currentPage == 1}">Previous</button></li>
      <li class="page-item"><button class="page-link" @click="getProjects(currentPage + 1)" :class="{'disabled' : currentPage == lastPage}">Next</button></li>
    </ul>
  </nav>
   
</template>

