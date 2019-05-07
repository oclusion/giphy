<template>
  <div id="app">
    <div class="controls-container">
      <input type="search" v-model="term" v-on:keyup.enter="onEnter" placeholder="Escribe">
      <i class="fa fa-search" @click=getGifs()></i>
    </div>
    <div class="gif-container" v-responsive="{
      small: el => el.width < 576, 
      medium: el => el.width < 768,
      large: el => el.width < 992,
      xlarge: el => el.width < 1200 }">
      <img v-for="gif in gifs" :src="gif" :key="gif.id">
    </div>
  </div>
</template>

<script>
import { ResponsiveDirective } from "vue-responsive-components"
export default {
  data() {
    return {
      term: "",
      gifs: []
    }
  },
  methods: {
    onEnter: function() {
       this.getGifs()
    },
    getGifs: function() {
      let apiKey = "kVn0Yq2YB8SvHdNybXAfjmEpntpMlS2B"
      let searchEndPoint = "//api.giphy.com/v1/gifs/search?"
      let limit = 20
      let url = `${searchEndPoint}&api_key=${apiKey}&q=${this.term}&limit=${limit}`

      fetch(url)
        .then(response => {
          return response.json()
        })
        .then(json => {
          this.buildGifs(json);
        })
        .catch(err => {
          console.log(err)
        })
    },
    buildGifs: function(json) {
      this.gifs = json.data.map(gif => gif.id).map(gifId => {
        return `//media.giphy.com/media/${gifId}/giphy.gif`
      })
    }
  },
  directives: {
    responsive: ResponsiveDirective
  }
}
</script>

<style>

body, html {
  padding: 0;
  margin: 0;
  height: 100vh;
  width: 100%;
  background: #07051a;
}

#app {
  height: 100vh;
  width: 100%;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

.controls-container {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    transition: all 1s;
    width: 50px;
    height: 50px;
    background: white;
    box-sizing: border-box;
    border-radius: 25px;
    border: 4px solid white;
    padding: 5px;
}

input {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;;
    height: 42.5px;
    line-height: 30px;
    outline: 0;
    border: 0;
    display: none;
    font-size: 1em;
    border-radius: 20px;
    padding: 0 20px;
}

.fa {
    box-sizing: border-box;
    padding: 10px;
    width: 42.5px;
    height: 42.5px;
    position: absolute;
    top: 0;
    right: 0;
    border-radius: 50%;
    color: #07051a;
    text-align: center;
    font-size: 1.2em;
    transition: all 1s;
}

.controls-container:hover {
    width: 200px;
    cursor: pointer;
}

.controls-container:hover input {
    display: block;
}

.controls-container:hover .fa {
    background: #07051a;
    color: white;
}

.gif-container {
  line-height: 0;
  -webkit-column-count: 5;
  -webkit-column-gap:   0px;
  -moz-column-count:    5;
  -moz-column-gap:      0px;
  column-count:         5;
  column-gap:           0px;
}

.gif-container img {
  width: 100% !important;
  height: auto !important;
}

.gif-container.xlarge {
  -moz-column-count:    4;
  -webkit-column-count: 4;
  column-count:         4;
}

.gif-container.large {
  -moz-column-count:    3;
  -webkit-column-count: 3;
  column-count:         3;
}

.gif-container.medium {
  -moz-column-count:    2;
  -webkit-column-count: 2;
  column-count:         2;
}

.gif-container.small {
  -moz-column-count:    1;
  -webkit-column-count: 1;
  column-count:         1;
}

</style>














