<script>
import { store } from '../store.js';

export default {
    name: 'ProjectCard',

    data() {
        return {
            store,
            contentMaxLength: 200,
        }
    },
    props: {
        cards: Array
    },
    methods: {
        truncateText(text) {
        if (text && text.length > this.contentMaxLength) {
        return text.substr(0, this.contentMaxLength) + '...';
        }
        return text;
      }
    }
};

</script>



<template>
    <div class="container">
        <div class="row">
            <div class="col-4" v-for="card in cards">
                <div class="card mt-5 text-center">
                    <img v-if="card.image" :src="`${this.store.baseUrl}/storage/${card.image}`" class="card-img-top">
                    <img v-else src="https://cdn.icon-icons.com/icons2/1462/PNG/512/120nophoto_100007.png" class="card-img-top"/>
                    <div class="card-body blogs">
                        <h5 class="card-title">{{card.title}}</h5>
                        <h5>{{card.type?.name}}</h5>
                        <span v-for="technology in card.technologies" class="badge rounded-pill text-bg-primary me-2">{{ technology.name }}</span>
                        <p class="card-text mt-3">{{truncateText(card.content)}}</p>
        
                        <router-link :to="{name: 'single-project', params: {slug: card.slug}}" class="btn btn-primary btn-list">
                            Vedi progetto completo
                        </router-link>
                              
                    </div>
                </div>
            </div>                   
        </div>
    </div>
     
</template>