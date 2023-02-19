<template>
  <a id="top"></a>
  <div class="bg-warning">
    <nav class="navbar">
      <div class="container bg-primary">
        <h1 class="display2 text-light fw-bolder position-relative top-100 start-50 translate-middle mt-5">MOVIEFREAK
          DATABASE</h1>


      </div>

    </nav>

    <div class="d-block gap-2 col-8 mt-3 mx-auto">
      <input type="text" class="form-control text-center" placeholder="Search Database..." v-model="search"
        @keyup.enter='find'/>
  
    </div>
    <div class="d-grid gap-2 col-2 mt-3 mx-auto pb-5">
      <button type="button" class="btn btn-primary" @click="find">Search</button>
    </div>


  </div>

  <div class="container "> 
  </div>

  <div class="container-fluid">
    <div class="col-12" v-for="results in outcome" :key="results.id">
      <div class="card mt-3" style="max-width: 150%">
        <div class="row">
          <div class="col-12">
            <img :src="this.img_link + results.poster_path" class="mx-auto d-block mt-2"
              :alt="results.original_title + this.cover_name" style="width: 150px;">
          </div>
          <div class="col-md-12">
            <div class="card-body">
              <h5 class="card-title text-center">{{ results.original_title }}</h5>
              <p class="card-text">{{ results.overview }}</p>
              <p class="card-text"><small class="text-muted">{{ results.release_date }}</small></p>


              <div class="col-12" v-for="code in results.genre_ids" :key="code">
                <!-- <div v-if="code.id === genres.id"></div> -->
                <p class="card-text"><small class="text-muted">{{ genres.filter(x => x.id === code).map(x => x.name)[0]
                }}</small></p>
              </div>

              <!-- genres.name -->


            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="mt-5">
    <nav aria-label="Page navigation example">
      <ul class="pagination justify-content-center">
        <li class="page-item" v-if="page > 1">
          <a href="#top" class="page-link" @click="subtract">Previous</a>
        </li>
        <li class="page-item disabled" v-if="page === 1">
          <a class="page-link" @click="subtract">Previous</a>
        </li>
        <li class="page-item">
          <a href="#top" class="page-link" @click="add" v-if="page > 0">Next</a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
// import vue from 'vue'
import axios from 'axios'
// import VueAxios from 'vue-axios'
// vue.use(axios)

export default {
    name: "HelloWorld",
    data() {
        return {
            cover_name: "movie cover",
            img_link: "https://www.themoviedb.org/t/p/w94_and_h141_bestv2",
            outcome: [],
            search: "",
            api: `pIvNRqDIX7YlwBksirwimUAqZ77d24Nm`,
            page: 0,
            genres: [
                { "id": 28, "name": "Action" },
                { "id": 12, "name": "Adventure" },
                { "id": 16, "name": "Animation" },
                { "id": 35, "name": "Comedy" },
                { "id": 80, "name": "Crime" },
                { "id": 99, "name": "Documentary" },
                { "id": 18, "name": "Drama" },
                { "id": 10751, "name": "Family" },
                { "id": 14, "name": "Fantasy" },
                { "id": 36, "name": "History" },
                { "id": 27, "name": "Horror" },
                { "id": 10402, "name": "Music" },
                { "id": 9648, "name": "Mystery" },
                { "id": 10749, "name": "Romance" },
                { "id": 878, "name": "Science Fiction" },
                { "id": 10770, "name": "TV Movie" },
                { "id": 53, "name": "Thriller" },
                { "id": 10752, "name": "War" },
                { "id": 37, "name": "Western" }
            ]
        };
    },
    methods: {
        add() {
            this.page += 1;
            // console.log(this.page)
            this.find();
        },
        subtract() {
            this.page -= 1;
            // console.log(this.page)
            this.find();
        },
        find() {
            this.page += 1;
            axios
                .get(`https://api.themoviedb.org/3/search/movie?api_key=f61395e368da24ae0732ed51327ff898&query=${this.search}&page=${this.page}&include_adult=false&year=${this.year}`)
                .then((response) => {
                console.log(response.data.results);
                this.outcome = response.data.results;
            });
        }
    }
    // async mounted () {
    // }
    // ,
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style></style>
