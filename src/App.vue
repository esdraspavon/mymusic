<template lang="pug">
  #app
    // img(src='dist/logo.png')
    h1 MyMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
      // li(v-for="artist in artists") {{artist.name}}
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries:[
        {name:'Argentina', value:'argentina'},
        {name:'Colombia', value:'colombia'},
        {name:'España', value:'spain'},
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists() {
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists){
          self.loading = false
          self.artists = artists
        })
        .catch(error => console.error(error))
        .finally(() => self.loading = false );
    }
  },
  mounted() {
    this.refreshArtists()
  },
  watch: {
    selectedCountry() {
      this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
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
  display grid
  list-style-type none
  padding 0
  grid-template-columns repeat(auto-fill, minmax(200px, 1fr))


li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
