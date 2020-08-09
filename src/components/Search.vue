<template>
	<div class="search" 
    v-bind:style="{ backgroundImage: 'url(' + require('@/assets/images/searchBackground.png') + ')', width:'95%'}">
      <div class="searchHead" v-bind:style="{marginTop:'50px',marginLeft:'20px', alignItems:left, flex:start, }" >
	        <span>
    				<span v-bind:style="{ fontFamily: 'system-ui',width: '100%', color:'#ffffff', fontSize:'40px', fontWeight:'bold',}">Welcome!</span>
			  	  <span v-bind:style="{marginTop:'10px', float:'left', width: '100%',fontFamily: 'system-ui', color:'#ffffff', fontSize:'20px', fontWeight:'bold'}">Worlds #1 search engine for Movie and TV Series.</span>
		  		</span>
			</div>
	
      <div class="container inputWrap" >
        <input 
        autocomplete="off" 
        v-on:keyup.enter="searchHandle" 
        type="text" 
        name="search" 
        placeholder="Movie or TV Series" 
        v-model="searchKey"/> 
        <a href="javascript:;" class="searchBtn" @click="searchHandle" >Search</a>
      </div>
    </div>
</template>

<script>
	import axios from 'axios' 
	export default {
		data() {
      return {
        searchKey : '',
        searchResults : [],
      }
    },
		methods : {
      searchHandle() {       
        this.$emit('search-results-data', this.searchKey)
        this.$router.push('/searchResults');      
      }
    }
	}
</script>