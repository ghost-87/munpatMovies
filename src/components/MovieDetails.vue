<script>
import axios from 'axios';
import moment from 'moment';

export default {
	name : 'MovieDetails',
	data() {
		return {
			movieDetails : [],
			baseUrl : 'https://fwemoviedb.herokuapp.com/3/movie/',
			apiKey : 'e800e93ef4806616964242bbd2619ae1',
			imgUrl : 'https://fwemoviedb.herokuapp.com/img/w500',
			movieGenres : [],
			movieTrailer : '',
			homepage : ''			
		}
	},
	methods : {
        format_date(value){
          if (value) {
            return moment(String(value)).format('MMMM DD, YYYY');
          }
        },

	},
	created() {
		const self = this;
		axios.get(`${this.baseUrl}${this.$route.params.movieID}?api_key=${this.apiKey}`)
		.then((res) => {
			self.movieDetails = res.data;
			self.movieGenres = res.data.genres;
			self.homepage = res.data.homepage;
		});
		console.log('apple', res);
		axios.get(`${this.baseUrl}${this.$route.params.movieID}/videos?api_key=${this.apiKey}`)
		.then((res) => {
			self.movieTrailer = res.data.results;
		});
	}
}
</script>

<template>
	<div class="movieDetails">
		<div class="mdBg">
			<div class="mdBgOverlay"></div>			
					<div class="mdBgImg" :style="{ backgroundImage: 'url(' + `${this.imgUrl}${this.movieDetails.poster_path}` + ')' }"></div>
		</div>
		<div class="movieDetailsList">
	    	<div class="content">
	    		<div class="poster"><img :src="imgUrl+movieDetails.poster_path" :alt="movieDetails.title" :title="movieDetails.title"></div>
	    		<div class="detailsText">
	    			<div v-if="searchResults[index].title" class="title">{{ searchResults[index].title }}</div>
	    			<div v-else class="title">{{ searchResults[index].name }}</div>
	    		<div v-if="searchResults[index].release_date" class="releaseDate">{{format_date(searchResults[index].release_date) }}</div>
	    		<div v-else class="releaseDate">{{format_date(searchResults[index].first_air_date) }}</div>
    				<div class="overview"><h2>{{ movieDetails.overview }}</h2></div>
	    		</div>
	    	</div>
	    </div>
	</div>
</template>