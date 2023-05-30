<script>
  import AppHeader from './components/AppHeader.vue';
  import axios from 'axios';

  export default {
    components: { AppHeader },

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
      truncateText(text) {
        if (text && text.length > this.contentMaxLength) {
        return text.substr(0, this.contentMaxLength) + '...';
        }
        return text;
      }
    },
    mounted() {
      this.getProjects();
    }


  };

</script>

<template>
  <AppHeader></AppHeader>
  
   <div class="container">
        <div class="row">

            <div class="col-4" v-for="project in projects">
                <div class="card">
                    <img v-if="project.image" :src="`${this.baseUrl}/storage/${project.image}`" class="card-img-top">
                    <img v-else src="https://cdn.icon-icons.com/icons2/1462/PNG/512/120nophoto_100007.png" class="card-img-top"/>
                    <div class="card-body">
                        <h5 class="card-title">{{project.title}}</h5>
                        <h5>{{project.type?.name}}</h5>
                        <p class="card-text">{{truncateText(project.content)}}</p>
                        <a href="#" class="btn btn-primary">Vedi post completo ... </a>
                    </div>
                </div>
            </div>

            <nav aria-label="Page navigation example">
                <ul class="pagination">
                    <li class="page-item"><button class="page-link" @click="getPosts(currentPage - 1)" :class="{'disabled' : currentPage == 1}">Previous</button></li>
                    <li class="page-item"><button class="page-link" @click="getPosts(currentPage + 1)" :class="{'disabled' : currentPage == lastPage}">Next</button></li>
                </ul>
            </nav>
            
        </div>
    </div>
  
</template>

<style lang="scss">
  @use './styles/general.scss';

</style>
