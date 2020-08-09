<script>
	import axios from 'axios';
	import moment from 'moment';

	export default {
		props : {
			searchKeyBus : {
				type : String
			}
		}	,
		data() {
			return {
				imgUrl : 'https://fwemoviedb.herokuapp.com/img/w780',
				searchResults : [],
	            // movies : [],
    	        // isVisible : true,
        	    // page : 1,
            	// loading : true,
	            // pageIDinc : 1,
    	        // pageIDdec : 0,      
        	    // getPage : '',
            	// reviews : [],
	            // reviewData : '',
    	        // rID : ''			
		}
	},
    methods : {

        // nextPage(event) {
        //     this.loading = true;
        //     this.page+=1;
        //     const self = this;
  		//       axios.get(`https://fwemoviedb.herokuapp.com/3/search/multi?api_key=e800e93ef4806616964242bbd2619ae1&query=${this.searchKeyBus}` + '&page=' + self.page + '')
        //     .then((res) => {
        //         self.movies = res.data.results;
        //         self.loading = false;                                
        //     });
        //     console.log('apple', res);
        //     this.pageIDinc++;
        //     this.pageIDdec++;
        // },
        // prevPage(event) {            
        //     this.loading = true;
        //     this.page-=1;
        //     this.pageIDdec = this.page;
        //     const self = this;
	  	//       axios.get(`https://fwemoviedb.herokuapp.com/3/search/multi?api_key=e800e93ef4806616964242bbd2619ae1&query=${this.searchKeyBus}` + '&page=' + self.page + '')
        //     .then((res) => {
        //         self.movies = res.data.results;
        //         self.loading = false;    
        //     });
        //     this.pageIDdec--;
        //     this.pageIDinc--;
        // },
        // goToPage() {
        //   this.loading = true;
        //   this.page = parseFloat(this.getPage);
        //   this.pageIDdec = parseFloat(this.getPage);
        //       this.pageIDinc = parseFloat(this.getPage);
        //   const self = this;
  	    //   axios.get(`https://fwemoviedb.herokuapp.com/3/search/multi?api_key=e800e93ef4806616964242bbd2619ae1&query=${this.searchKeyBus}` + '&page=' + self.page + '')
        //   .then((res) => {
        //       self.movies = res.data.results;
        //       self.loading = false;
        //       self.$router.push(`/page/${self.getPage}`);
        //   });
        // },
        format_date(value){
          if (value) {
            return moment(String(value)).format('MMMM DD, YYYY');
            console.log('formatted date', value);
          }
        },
    },   
		
		created() {
			console.log('i a m here');
			const self = this;
	      axios.get(`https://fwemoviedb.herokuapp.com/3/search/multi?api_key=e800e93ef4806616964242bbd2619ae1&query=${this.searchKeyBus}`)
    	  .then((res) => {
			  console.log('search value', res);
       		self.searchResults = res.data.results;
      	}); 
		}
	}
</script>

<template>
  <div class="popular containers">
  	<div v-if="loading" class="loading"><img src="../assets/images/loading.gif"></div>
    <div class="searchCardsDeck">
    	<div class="card" v-for="(movie, index) in searchResults">
    		<router-link :to="`/movies/${searchResults[index].id}`" target="_blank">
	   			<div v-if="imgUrl + searchResults[index].backdrop_path" class="poster"><img :src="imgUrl + searchResults[index].backdrop_path"></div>
	   			<div v-else class="poster">
					<img src="../assets/images/no-image.png" alt="no image available" title="no image available" />
				</div>
	    	</router-link>
	    	<div class="cardRight">
	    		<router-link :to="`/movies/${searchResults[index].id}`" target="_blank">
	    			<div v-if="searchResults[index].title" class="title">{{ searchResults[index].title }}</div>
	    			<div v-else class="title">{{ searchResults[index].name }}</div>
	    		</router-link>
	    		<div class="overview">{{ searchResults[index].overview.slice(0,235) }}...</div>
	    		<div v-if="searchResults[index].release_date" class="releaseDate">{{format_date(searchResults[index].release_date) }}</div>
	    		<div v-else class="releaseDate">{{format_date(searchResults[index].first_air_date) }}</div>
	  			<router-link :to="`/movies/${searchResults[index].id}`" target="_blank">
	    		</router-link>
    		</div>
    	</div>
    </div>



  </div>
</template>