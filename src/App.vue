<template lang='pug'>
  #app
    img(src='./assets/logo.png')
    h1 Platzi Music
    select(v-model='selectedCountry')
      option(v-for='country in countries' :value='country.value') {{ country.name }}
    spinner(v-show='loading')
    ul
      artist(v-for='artist in artists' v-bind:artist='artist' v-bind:key='artist.mbid')
</template>

<script>
  import getArtists from './api';
  import Artist from './components/Artist.vue'
  import Spinner from './components/Spinner.vue'
  export default {
    name: 'app',
    data () {
      return {
        artists: [],
        selectedCountry: 'spain',
        loading: true,
        countries: [
          {name: 'Venezuela', value:'venezuela'},
          {name: 'España', value: 'spain'},
          {name: 'Argentina', value: 'argentina'},
          {name: 'Colombia', value: 'colombia'},
        ]
      }
    },
    components: {
      Artist,
      Spinner
    },
    methods: {
      refreshArtists: function () {
        const self = this;
        this.loading = true;
        this.artists = [];
        
        getArtists(this.selectedCountry)
          .then(function (artists) {
            self.artists = artists;
            self.loading = false;
          })
      }
    },
    mounted: function () {
      this.refreshArtists();
    },
    watch: {
      selectedCountry: function () {
        this.refreshArtists();
      }
    }
  }
</script>

<style lang='stylus' scoped>
  #app 
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin-top 60px
  h1, h2 
    font-weight normal
  ul 
    list-style-type none
    padding 0
  li 
    display inline-block
    margin 0 10px
  a 
    color #42b983
</style>
