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
    mounted() {
        const slug = this.$route.params.slug;

        console.log("slug:" + slug);

        axios.get(`${this.store.baseUrl}/api/project/${slug}`)
            .then(response => {
                console.log(response);
                this.project = response.data.project;
            });

    }
}
</script>

<template>
    <div class="container">
        <div class="row">
            <div class="col-8">
                <div class="card mt-4">
                    
                    <div class="card-body text-center" v-if="project">
                        <h2 class="card-title text-center mb-3">{{project.title}}</h2>
                        <h5 class="card-title text-center mb-3">{{ project.slug }}</h5>
                        <h4 class="text-center mb-4">{{ project.type?.name }}</h4>
                        <img v-if="project.image" :src="`${this.store.baseUrl}/storage/${project.image}`" class="card-img-top">
                        <img v-else src="https://cdn.icon-icons.com/icons2/1462/PNG/512/120nophoto_100007.png" class="card-img-top"/>
                        <h5 class="card-title mt-3">{{ project.link_project }}</h5>
                        <h5 class="card-title mt-3">{{ project.content }}</h5>

                    </div>
                </div>

            </div>
        </div>

    </div>
</template>
