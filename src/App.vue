<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 DiscoverMusic
    h2 ¡Descubre los artistas trending!
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue'
import getArtists from './api'
import spinner from './components/spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        { name: 'Venezuela', value: 'venezuela' },
        { name: 'Colombia', value: 'colombia' },
        { name: 'Argentina', value: 'argentina' },
        { name: 'España', value: 'spain' },
      ],
      selectedCountry: 'venezuela',
      loading: true
    }
  },
  components: {
    Artist,
    spinner
  },
  methods: {
    refreshArtists() {
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.loading = false
          self.artists = artists
        })
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

body
  background-color: #e5e5e5

ul
  background-color: #ff6347
  list-style-type: decimal;

ul li
  border: 1px solid #a52323
  border-radius: 1%
  display: flex;
  padding: 1em;

ul li.artist img
  border-radius: 50%

h1, h2
  font-weight normal

h2
  font-size: 20px

ul
  list-style-type none
  padding 0

li
  display inline

a
  color #303030
</style>
