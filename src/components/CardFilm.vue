<script>
import axios from 'axios';
import { store } from '../store';
export default {
    data() {
        return {
            store,
            hoveredClass: 'hovered',
            isHovered: false,
        }
    },
    props: {
        movie: Object,
    },
    methods: {
        flagLanguage(movie) {
            let language = movie.original_language;
            if (language == 'en') {
                language = 'gb';
            } else if (language == '') {
                language = 'Sconosciuta';
            } else if (language == 'ja') {
                language = 'jp';
            } else if (language == 'zh') {
                language = 'cn';
            };
            return language;
        },
        imagePoster(movie) {
            return this.store.APIimage + movie.poster_path;
        },
        changeHovered() {
            this.isHovered = true;
        },
        notHovered() {
            this.isHovered = false;
        },

        getGenres(genre) {
            let genreArray = [];
            for (let index = 0; index < store.genresListMovie.length; index++) {
                if (genre == store.genresListMovie[index].id) {
                    genreArray += store.genresListMovie[index].name;
                }
            }
            return genreArray;
        },
    },
}
</script>




<template>
    <div id="film-card" @mouseover="changeHovered()" @mouseleave="notHovered()">
        <div class="card-text">
            <h3>{{ movie.title }}</h3>
            <div><em>{{ movie.original_title }}</em></div>
            <div>Lingua Originale: <i :class="`fi fi-${flagLanguage(movie)} fis`"></i></div>
            <div>
                Voto Film:
                <i v-for="star in Math.ceil(movie.vote_average / 2)" class="fas fa-star"></i>
                <i v-for="whiteStar in 5 - Math.ceil(movie.vote_average / 2)" class="fas fa-star" id="white-star"></i>
            </div>
            <div id="genres">Genere:
                <span v-for="genre in movie.genre_ids"> {{ getGenres(genre) }}</span>
            </div>
        </div>
        <img :src=imagePoster(movie) alt="" class="image" :class="isHovered ? 'hovered' : 'image'">

    </div>
</template>



<style lang="scss" scoped>
#film-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    min-width: 250px;
    max-width: 250px;
    text-align: center;
    gap: .5em;
    border: 2px solid black;
    border-radius: 6px;
    padding: 0.5em 20px;
    background-color: rgba(0, 0, 0, .8);
    height: 350px;

    .card-text {
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
        gap: .5em;
        position: relative;
        z-index: 2;

        .fa-star {
            color: green;
        }

        #white-star {
            color: white;
        }

        #genres {
            font-size: .8em;


            span {
                margin-right: .3em;
                display: inline-block;

            }
        }
    }

    .image {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        opacity: 1;
        z-index: 3;
        border-radius: 8px;
    }

    .hovered {
        opacity: .2;
        z-index: 1;
    }
}
</style>
