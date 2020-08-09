<script>
import axios from 'axios';
import moment from 'moment';

export default {
    name : 'PopularMovies',
    data() {
        return {
            apiKey : 'e800e93ef4806616964242bbd2619ae1',
            baseUrl : 'https://fwemoviedb.herokuapp.com/3/',
            imgUrl:'https://fwemoviedb.herokuapp.com/img/w500',
            movies : [],
            isVisible : true,
            page : 1,
            loading : true,
            pageIDinc : 1,
            pageIDdec : 0,      
            getPage : '',
            reviews : [],
            reviewData : '',
            rID : ''
        }
    },  
    methods : {

        nextPage(event) {
            this.loading = true;
            this.page+=1;
            const self = this;
            axios.get('https://fwemoviedb.herokuapp.com/3/movie/popular?api_key=e800e93ef4806616964242bbd2619ae1&page=' + self.page + '')
            .then((res) => {
                self.movies = res.data.results;
                self.loading = false;                                
            });
            console.log('apple', res);
            this.pageIDinc++;
            this.pageIDdec++;
        },
        prevPage(event) {            
            this.loading = true;
            this.page-=1;
            this.pageIDdec = this.page;
            const self = this;
            axios.get('https://fwemoviedb.herokuapp.com/3/movie/popular?api_key=e800e93ef4806616964242bbd2619ae1&page=' + self.page + '')
            .then((res) => {
                self.movies = res.data.results;
                self.loading = false;    
            });
            this.pageIDdec--;
            this.pageIDinc--;
        },
        goToPage() {
          this.loading = true;
          this.page = parseFloat(this.getPage);
          this.pageIDdec = parseFloat(this.getPage);
              this.pageIDinc = parseFloat(this.getPage);
          const self = this;
          axios.get('https://fwemoviedb.herokuapp.com/3/movie/popular?api_key=e800e93ef4806616964242bbd2619ae1&page=' + self.page + '')
          .then((res) => {
              self.movies = res.data.results;
              self.loading = false;
              self.$router.push(`/page/${self.getPage}`);
          });
        },
        format_date(value){
          if (value) {
            return moment(String(value)).format('MMMM DD, YYYY');
            console.log('formatted date', value);
          }
        },
    },   
    created() {              
      this.page = this.pageIDinc;
      const self = this;
      axios.get('https://fwemoviedb.herokuapp.com/3/movie/popular?api_key=e800e93ef4806616964242bbd2619ae1&page=' + self.page + '')
      .then((res) => {
          console.log('banana', 'res');
          console.log(res.data);
          self.movies = res.data.results;
          self.loading = false;           
      }); 

      this.reviews = reviewsJson;  
      this.rID = this.$route.params.reviewID
      this.rID -=1;
      this.reviewData = this.reviews[this.rID];        
    }
}
</script>

<template>
  <div class="popular">
    <appSlider></appSlider>
    <div class="banner banner728"></div>
    <appTrailers></appTrailers>
    <div class="pageHeadline"><h1>Popular Movies & TV Series</h1></div>    
    <div v-if="loading" class="loading"><img src="../assets/images/loading.gif" alt="Loading" title="Loading"></div> 

    <div class="cardsDeck" v-if="!loading">
      <div class="card" v-for="(movie, index) in movies">
        <router-link :to="`/movies/${movies[index].id}`" target="_blank">
          <div class="poster"><img :src="imgUrl + movies[index].backdrop_path" :alt="movies[index].title" :title="movies[index].title"></div>
            <router-link :to="`/movies/${movies[index].id}`" target="_blank">
            <div class="movieInfo">
              <div class="title">{{ movies[index].title.slice(0,50) }}</div>
              <div class="releaseDate">{{ format_date(movies[index].release_date) }}</div>
              </div>
          </router-link>
        </router-link>
      </div>
    </div>

    <div class="pagination">
      <ul class="paginationBtns">
        <li>
            <router-link :to="`/page/${pageIDdec}`" v-show="this.page > 1" @click.native="prevPage($event)" title="Prev">
                <i class="icon ion-md-arrow-back"></i>
            </router-link> 
        </li>
        <li>
            <a href="javascript:;">{{ this.page }}</a>
        </li>
        <li>
            <router-link :to="`/page/${pageIDinc + 1}`" v-show="this.page < 1000" @click.native="nextPage($event)" title="Next">
                <i class="icon ion-md-arrow-forward"></i>
            </router-link>
        </li>
      </ul>
      <div class="currentPage">
        <div>Go to : </div>
        <div><input type="text" name="goToPage" v-model="getPage" @keypress.enter="goToPage"></div>
        <div>/ 1000</div>
        <div v-if="getPage"><a href="javascript:;" @click="goToPage"><i class="ion-md-return-right"></i></a></div>
      </div>
    </div>

  </div>
</template>