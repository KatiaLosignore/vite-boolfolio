<script>
import axios from 'axios';
import { store } from '../store.js';

export default {
    name: 'SingleProject',
    data() {
        return {
            store,
            project: null
        }
    },
    methods: {
        getProject() {
            const slug = this.$route.params.slug;

            console.log("slug:" + slug);

            axios.get(`${this.store.baseUrl}/api/project/${slug}`)
            .then(response => {
                console.log(response);
                if (response.data.success == true) {
                    this.project = response.data.project;
                } else {
                    this.$router.push({name: 'not-found'});
                }
            });

        }
    },
    created() {
        this.$watch(
            () => this.$route.params,
            (toParams, previousParams) => {
                this.getProject();
            }
        )
    },
    mounted() {
        this.getProject();
    }
   
}
</script>


<template>
    <div class="container">
        <div class="row py-5">
            <div class="col-8">
                <div class="card rounded-5 bg-info-subtle">
                    
                    <div class="card-body text-center card-single" v-if="project">
                        <h2 class="card-title text-center mb-3">{{project.title}}</h2>
                        <h5 class="card-title text-center mb-3">Slug: {{ project.slug }}</h5>
                        <h4 class="text-center mb-4">Type: {{ project.type?.name }}</h4>
                        <span v-for="technology in project.technologies" class="badge rounded-pill text-bg-primary me-2 mb-3">{{ technology.name }}</span>
                        <img v-if="project.image" :src="`${this.store.baseUrl}/storage/${project.image}`" class="card-img-top">
                        <img v-else src="https://cdn.icon-icons.com/icons2/1462/PNG/512/120nophoto_100007.png" class="card-img-top"/>
                        <h5 class="card-title mt-3">Link project: {{ project.link_project }}</h5>
                        <h5 class="card-title mt-3">{{ project.content }}</h5>

                        <router-link :to="{name: 'single-project', params: {slug: 'est-in-magnam'}}" class="btn btn-primary mt-3 mb-5">
                             Vedi progetto Est in magnam
                        </router-link>

                    </div>
                    <div class="text-center mt-2" v-else>
                        <img src="/loader.gif" alt="Caricamento in corso"/>
                    </div>
                </div>

            </div>
        </div>

    </div>
</template>
