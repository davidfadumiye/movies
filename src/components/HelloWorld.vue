<template>
  <a id="top"></a>
  <div class="bg-warning">
    <nav class="navbar">
      <div class="container-fluid bg-primary">
        <h1 class="display2 text-light fw-bolder position-relative top-100 start-50 translate-middle mt-5">
          Movie<span>base</span></h1>


      </div>

    </nav>

    <div class="d-block gap-2 col-8 mt-3 mx-auto">
      <input type="text" class="form-control text-center" placeholder="Search Database..." v-model="search"
        @keyup.enter='find'/>

    </div>
    <div class="d-grid gap-2 col-2 mt-3 mx-auto pb-5">
      <button type="button" class="btn btn-primary" @click="find" v-if="year === ''">Search</button>
      <button type="button" class="btn btn-primary" @click="filteryear" v-if="year !== ''">Search</button>
    </div>


  </div>

  <div class="container-fluid">
    <div class="row">
      <div class="">
        <button @click="show = !show" class="btn btn-light my-2 px-5 mx-auto">Filter Results</button>
        <Transition duration="550" name="nested">
          <div v-if="show" class="outer">

            <div class="d-grid col-2">
              <label class="form-label">Year</label>
              <input type="text" placeholder="year" class="form-control" v-model="year"
                 @focus="filteryear">
              <button type="text" class="form-control btn btn-primary" @click="find">Reset</button>
            </div>

            <div class="d-grid col-2">
              <label class="form-label">Genre</label>
              <select class="form-select" @change="picked" v-model="selected">
                <option value="" selected disabled>Select genre...</option>
                  <option :value="genre.name" v-for="genre in genres" :key="genre.id">{{ genre.name }}</option>

                
              </select>
              <!-- <input type="text" placeholder="year" class="form-control" v-model="year"
                 @focus="filteryear"> -->
              <button type="text" class="form-control btn btn-primary" @click="find">Reset</button>
            </div>

          </div>
        </Transition>
      </div>
    </div>
  </div>

  <div class="container ">
  </div>

  <div class="container-fluid ops">
    <div class="col-12" v-for="results in outcome" :key="results.id">
      <div class="card mt-3 bg-secondary bg-gradient" style="max-width: 150%">
        <div class="row">
          <div class="col-12">
            <img :src="this.img_link + results.poster_path" class="mx-auto d-block mt-2 fs-6 fw-lighter"
              :alt="results.original_title + this.cover_name" style="width: 150px;">
          </div>
          <div class="col-md-12">
            <div class="card-body">
              <h5 class="card-title text-center">{{ results.original_title }}</h5>
              <p class="card-text fw-light">{{ results.overview }}</p>
              <p class="card-text"><small class="text-muted">{{ results.release_date }}</small></p>


              <div class="d-flex flex-row bd-highlight mb-3">
                <div class="p-2 bd-highlight">
                  <p>Genre:</p>
                </div>

                  <div class="p-2 bd-highlight" v-for="code in results.genre_ids" :key="code">
                    <!-- <div v-if="code.id === genres.id"></div> -->
                    <p class="card-text"><small class="text-muted">{{ genres.filter(x => x.id === code).map(x =>
                      x.name)[0]
                    }}</small></p>
                  </div>

              </div>

              <!-- genres.name -->


            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="mt-5" @loadeddata="find">
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
      show: false,
      cover_name: " movie cover",
      img_link: "https://www.themoviedb.org/t/p/w94_and_h141_bestv2",
      outcome: [],
      search: "",
      api: `pIvNRqDIX7YlwBksirwimUAqZ77d24Nm`,
      page: 0,
      year: '',
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
      ],
      selected: ''
    };
  },
  methods: {
    // toggle() {
    //   this.pressed = !this.pressed
    //   console.log(this.pressed)
    // },
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
      this.page = 1;
      axios
        .get(`https://api.themoviedb.org/3/search/movie?api_key=f61395e368da24ae0732ed51327ff898&query=${this.search}&page=${this.page}&include_adult=false`)
        .then((response) => {
          console.log(`https://api.themoviedb.org/3/search/movie?api_key=f61395e368da24ae0732ed51327ff898&query=${this.search}&page=${this.page}&include_adult=false`)
          console.log(response.data.results);
          this.outcome = response.data.results;

        });
    },
    filteryear() {
      this.page = 1
      // this.year = Number(this.year)
      axios
        .get(`https://api.themoviedb.org/3/search/movie?api_key=f61395e368da24ae0732ed51327ff898&query=${this.search}&page=${this.page}&include_adult=false&year=${this.year}`)
        .then((response) => {
          console.log(`https://api.themoviedb.org/3/search/movie?api_key=f61395e368da24ae0732ed51327ff898&query=${this.search}&page=${this.page}&include_adult=false&year=${this.year}`)
          console.log(response.data.results);
          this.outcome = response.data.results;
        });
    },
    picked() {
      console.log(this.selected)
    }
  }
  // async mounted () {
  // }
  // ,
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.outer,
.inner {
  background: #eee;
  padding: 10px;
  min-height: 50px;
}

.inner {
  background: #ccc;
}

.nested-enter-active,
.nested-leave-active {
  transition: all 0.3s ease-in-out;
}

/* delay leave of parent element */
.nested-leave-active {
  transition-delay: 0.25s;
}

.nested-enter-from,
.nested-leave-to {
  transform: translateY(30px);
  opacity: 0;
}

/* we can also transition nested elements using nested selectors */
.nested-enter-active .inner,
.nested-leave-active .inner {
  transition: all 0.3s ease-in-out;
}

/* delay enter of nested element */
.nested-enter-active .inner {
  transition-delay: 0.25s;
}

.nested-enter-from .inner,
.nested-leave-to .inner {
  transform: translateX(30px);

  /* Hack around a Chrome 96 bug in handling nested opacity transitions.
This is not needed in other browsers or Chrome 99+ where the bug
has been fixed. */

  opacity: 0.001;
}</style>
