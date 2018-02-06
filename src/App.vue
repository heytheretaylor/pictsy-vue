<template>
  <v-app id="app" v-resize="onResize">
    <v-toolbar dark>
      <v-toolbar-title>
        Pictsy!
      </v-toolbar-title>
      <v-spacer></v-spacer>
        <v-menu :close-on-content-click="false" offset-y>
          <v-btn color="green" slot="activator">
            About
          </v-btn>
          <v-list>
            <v-list-tile>
              <v-list-tile-title>
                Made by Taylor Mason... Quickly
              </v-list-tile-title>
            </v-list-tile>
          </v-list>
        </v-menu>
    </v-toolbar>
    <div id="menu-sentence">
      <span v-if="sectionSelect.code != 'user'">The</span>
      <v-menu offset-y>
        <span slot="activator" style="text-decoration: underline;">{{` ${sectionSelect.name} `}}</span>
        <v-list>
          <v-list-tile v-for="section in sections" :key="section.code" @click="selectSection(section)">
            <v-list-tile-title>{{ section.name }}</v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-menu>
      <span v-if="sectionSelect.code != 'user'"> images on the Internet, sorted by</span>
      <span v-else> images, sorted by  </span>
      <v-menu offset-y>
        <span slot="activator" style="text-decoration: underline;">{{" " + sortSelect.name + " "}}</span>
        <v-list>
          <v-list-tile v-for="sort in sorts" :key="sort.code" @click="selectSort(sort)">
            <v-list-tile-title>{{ sort.name }}</v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-menu>
    </div>
	  <v-container id="main-grid">
	    <Post v-for="post in responseData" :post="post" :key="post.id"/>
	  </v-container>
  </v-app>
</template>

<script>
import Post from './components/Post'
import axios from 'axios'

export default {
  name: 'App',
  components: {
	Post
  },
  data (){
    return {
      search: "",
      clientId: "1c47a6aa2a1e3d6",
      responseData: {},
      sections: [
        {code: "hot", name: "Most Viral"},
        {code: "top", name: "Most Popular"},
        {code: "user", name: "User Submitted"},
      ],
      sorts: [
        {code: "viral", name: "Popularity"},
        {code: "top", name: "Highest Scoring"},
        {code: "time", name: "Newest First"},
      ],
      windowSize: {
        x: 0,
        y: 0
      },
      sectionSelect: {
        code: "hot",
        name: "Most Viral"
      },
      sortSelect: {
        code: "viral",
        name: "Popularity"
      }
    }
  },
  mounted: function(){
    this.apiCall();
    this.onResize();
  },
  computed:{
  },
  methods: {
    apiCall: function(params){
      var _self = this;
      axios({
        method: 'get',
        url: `https://api.imgur.com/3/gallery/${this.sectionSelect.code}/${this.sortSelect.code}` + this.search,
        headers: { 'authorization': 'Client-ID ' + this.clientId }
      }).then(function(response) {
        console.log(response.data.data);
        _self.responseData = response.data.data
      }).catch(function(error) {
        console.log(error);
      });
    },
    onResize: function(){
      this.windowSize = { x: window.innerWidth, y: window.innerHeight }
    },
    selectSection: function(section){
      this.sectionSelect = section;
      this.apiCall();
    },
    selectSort: function(sort){
      this.sortSelect = sort;
      this.apiCall();
    }
  }
}
</script>

<style>
#app {
  
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #141518;
  /* margin-top: 60px; */
}

#main-grid{
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-gap: 10px;
  background-color: #2C2F34;
  /* width: 1000px; */
  padding: 21px;
  
}

#menu-sentence{
  display: flex;
  justify-content: center;
  font-size: 22px;
  color: white;
  margin: 10px;
}

#menu-sentence span{
  white-space:pre;
}
</style>
