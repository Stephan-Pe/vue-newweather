<template>
  <v-app>
    <v-app-bar class="indigo darken-4"
      app
      >
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>The Weather Girls</v-toolbar-title>
    </v-app-bar>
    <v-navigation-drawer app class="indigo darken-4" v-model="drawer">
      <v-list-item v-for="item in items"
      :key="item.title"
      link
      :to="item.path">
      <v-list-item-content>
        <v-list-item-title>{{item.title}}</v-list-item-title>
        </v-list-item-content>
        </v-list-item>
    </v-navigation-drawer>
    <v-main v-bind:class="backgroundImage">
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
  import { mapGetters } from 'vuex'
  export default {
    name: 'App',
    data () {
      return {
        drawer: false,
        items: [
           {title: 'Home', path: '/'},
          {title: 'Search', path: '/search'}
        ]
      }
    },
    computed: {
      ...mapGetters({
        backgroundImage: "backgroundImage"
      })
    },
    mounted: function () {
      this.$store.dispatch('fetchWeatherForStore', this.$store.state.city)
    }
  }
</script>

<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    font-size: 1.6rem;
  }
  input {
    font-size: 1.6rem !important;
  }
  .cold {
    background-attachment: fixed;
     background: url("~@/assets/snowboard.jpg"), radial-gradient(circle, rgba(128, 136, 143, 0.5) 35%, rgba(48, 51, 51, 0.5) 100%);
  background-blend-mode: multiply;
  background-position: center;
  background-repeat: no-repeat;
  }

  .hot {
    background-attachment: fixed;
    background: url("~@/assets/beach.jpg"),
    radial-gradient(
      circle,
      rgba(5, 84, 153, 0.5) 35%,
      rgba(11, 165, 196, 0.5) 100%
    );
  background-blend-mode: multiply;
  background-position: center;
  background-repeat: no-repeat;
  }
 .v-sheet.v-app-bar.v-toolbar {
    z-index: 99;
  }
  .v-navigation-drawer__content {
    margin-top: 70px;
    z-index: 2;
  }
  
</style>
