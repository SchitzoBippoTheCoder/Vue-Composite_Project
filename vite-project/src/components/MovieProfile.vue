<template>
    <div id="movieImage" ref="movieImage">
    </div>
    <div id="movieProfile">
        <img id="poster" :src="poster">
        <h1 id="title"> {{ title }}</h1>
        <h3 id="tagline">{{ tagline }}</h3>
        <h3 id="status">{{ status }}</h3>
        <p id="popularity">{{ popularity }}</p>
        <p id="voteAverage">{{ voteAverage }}</p>
        <p id="voteCount">{{ voteCount }}</p>
        <p id="budget">{{ budget }}</p>
        <p id="overview">{{ overview }}</p>

    </div>
    <iframe id="trailer" :src="trailer"></iframe>

    <select v-model="dropdownMenu" name="listOfMovies" id="dropdownMenu" @change="">
        <option v-for="option in options" :value=option.value>{{ option.text }}</option>
    </select>

    <button id="getButton" @click="getMovieData()">Get</button>

    <button id="searchButton" @click="search()">Search</button>

    <input type="text" id="searchBar" ref="searchBar">

</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const dropdownMenu = ref();
const searchBar = ref(null);

const movieImage = ref(null);
const poster = ref(null);
const title = ref(null);
const tagline = ref(null);
const status = ref(null);
const popularity = ref(null);
const voteAverage = ref(null);
const voteCount = ref(null);
const budget = ref(null);
const overview = ref(null);
const trailer = ref(null);

let options = ref([
    {text: "John Wick", value: "245891"},
    {text: "I See You", value: "524251"},
    {text: "Batman: Under the Red Hood", value: "40662"},
    {text: "Prisoners", value: "146233"},
    {text: "Jojo Rabbit", value: "515001"},
    {text: "Justice League: The Flashpoint Paradox", value: "183011"},
    {text: "살인의 추억", value: "11423"},
    {text: "Barbie in the 12 Dancing Princesses", value: "13002"},
    {text: "The Dark Knight", value: "155"},
    {text: "Barbie: Princess Charm School", value: "73456"},
]);

function getMovieData() {
    let search = axios.get(`https://api.themoviedb.org/3/movie/${dropdownMenu.value}`, {
        params: {
            api_key: "e06cb446302dcf3a3cb1358720141aad",
            append_to_response: "videos",
        }
    })

    let searchResult = search.then((movieData) => {
        movieImage.value.style.backgroundImage = `url(https://image.tmdb.org/t/p/w500/${movieData.data.backdrop_path})`;
        poster.value = "https://image.tmdb.org/t/p/w500" + movieData.data.poster_path;
        title.value = movieData.data.original_title;
        tagline.value = movieData.data.tagline;
        status.value = `${movieData.data.status} ~ ${movieData.data.release_date}`;
        popularity.value = `Popularity: ${movieData.data.popularity}`;
        voteAverage.value = `Vote Average: ${movieData.data.vote_average}`;
        voteCount.value = `Vote Count: ${movieData.data.vote_count}`;
        budget.value = `Budget: $${movieData.data.budget}`;
        overview.value = movieData.data.overview;
        trailer.value = "https://www.youtube.com/embed/" + (movieData.data.videos.results.filter((trailer) => trailer.type === "Trailer")).at(0).key;
    })
}

function search() {
    let apiKey = "e06cb446302dcf3a3cb1358720141aad";

    let searchParam = searchBar.value.value;

    let _searchParam = axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&language=en-US&page=1&include_adult=false&query=${searchParam}`, {

    })

    let _searchParamResult = _searchParam.then((finalResult) => {

        options.value = [];

        for (let count = 0; count < finalResult.data.results.length; count++) {
            options.value.push(
                {text: finalResult.data.results[count].original_title, value: finalResult.data.results[count].id}
            )
        }
    })


}

</script>

<style scoped>
body {
    background-color: black;
}

#dropdownMenu {
    font-family: 'Courier New', Courier, monospace;
    font-weight: normal;

    color: white;
    background-color: rgb(22, 22, 22);

    outline: rgb(22, 22, 22);

    width: 400px;
    height: 24px;

    border-style: solid;
    border-radius: 100px;

    margin-left: 5px;

    margin-top: 20px;
    margin-bottom: 15px;

    padding-left: 5px;

}

#searchBar {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    color: white;
    font-size: medium;

    background-color: rgb(22, 22, 22);

    outline: rgb(22, 22, 22);

    top: 717px;
    left: 450px;

    height: 17px;
    width: 350px;

    border-radius: 25px;

    padding-left: 10px;
}

#searchButton {
    font-family: 'Courier New', Courier, monospace;
    color: white;
    font-size: medium;

    background-color: rgb(7, 7, 7);

    position: absolute;

    top: 717px;
    left: 785px;

    height: 24px;

    border-style: solid;
    border-width: 0px;
    border-radius: 50px;

    padding-top: 2px;
    padding-right: 15px;
    padding-bottom: 2px;
    padding-left: 15px;

    z-index: 99;
}

#getButton {
    font-family: 'Courier New', Courier, monospace;
    color: white;
    font-size: medium;

    background-color: rgb(7, 7, 7);

    position: absolute;

    top: 718px;
    left: 365px;

    height: 24px;

    border-style: solid;
    border-width: 0px;
    border-radius: 50px;

    padding-top: 2px;
    padding-right: 15px;
    padding-bottom: 2px;
    padding-left: 15px;
}

#movieImage {
    background-image: url(https://image.tmdb.org/t/p/w500/ff2ti5DkA9UYLzyqhQfI2kZqEuh.jpg) no-repeat center;
    background-size: cover;

    background-color: grey;
    background-blend-mode: multiply;

    filter: blur(10px);
    filter: brightness(25%);
    z-index: -1;

    border-radius: 10px;

    width: 97vw;
    height: 690px;

    margin-left: 5px;
}

#movieProfile #poster {
    position: absolute;

    top: 10px;

    padding-top: 25px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 30px;

    width: 200px;
    height: 300px;

    border-radius: 35px;

    display: inline;
}

#movieProfile #title {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    color: white;

    top: 10px;
    left: 240px;

    padding-top: 10px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 25px;
}

#movieProfile #tagline {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    font-weight: lighter;
    font-style: italic;
    color: white;

    top: 65px;
    left: 240px;

    padding-top: 0px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 25px;
}

#movieProfile #status {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    color: white;

    top: 115px;
    left: 240px;

    padding-top: 0px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 25px;

}

#movieProfile #popularity {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    color: white;

    top: 165px;
    left: 240px;

    padding-top: 0px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 25px;
}

#movieProfile #voteAverage {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    color: white;

    top: 185px;
    left: 240px;

    padding-top: 0px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 25px;
}

#movieProfile #voteCount {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    color: white;

    top: 205px;
    left: 240px;

    padding-top: 0px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 25px;
}

#movieProfile #budget {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    color: white;

    top: 245px;
    left: 240px;

    padding-top: 0px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 25px;
}

#movieProfile #overview {
    position: absolute;

    font-family: 'Courier New', Courier, monospace;
    line-height: 145%;
    color: white;

    top: 345px;
    left: 15px;

    width: 665px;

    padding-top: 0px;
    padding-right: 25px;
    padding-bottom: 25px;
    padding-left: 25px;
}

#trailer {
    position: absolute;

    margin-top: 25px;
    margin-right: 10px;

    right: 30px;
    top: 340px;

    aspect-ratio: 16/9;

    height: 300px;
    width: 450px;

    border-radius: 10px;
}
</style>
