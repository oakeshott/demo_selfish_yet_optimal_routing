<template>
  <div id="app">
  <v-toolbar>
    <v-toolbar-title>DEMO: Selfish yet optimal routing</v-toolbar-title>
    <v-spacer></v-spacer>
    <v-toolbar-items class="hidden-sm-and-down">
      <v-btn icon href="/">
        <v-icon>home</v-icon>
      </v-btn>
    </v-toolbar-items>
  </v-toolbar>
  <v-container grid-list-md text-xs-center>
    <v-layout row wrap>
      <v-flex xs6>
        <v-card>
          <SelfishRouting/>
        </v-card>
      </v-flex>
      <v-flex xs6>
        <v-card>
          <SelfishYetOptimalRouting/>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld'
import SelfishRouting from './components/SelfishRouting.vue'
import SelfishYetOptimalRouting from './components/SelfishYetOptimalRouting.vue'

export default {
  name: 'App',
  components: {
    SelfishRouting,
    SelfishYetOptimalRouting
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
  /* margin-top: 60px; */
}
path {
  fill: none;
}
</style>
