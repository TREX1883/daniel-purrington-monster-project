<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Monster</span>
        <span class="font-weight-light">Slayer</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
      >
        <span class="mr-2">Beta Release</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <HelloWorld/>
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import { 
  VApp,
  VAppBar, 
  VToolbarTitle,  
  VContent 
  } from 'vuetify/lib'

export default {
  name: 'App',
  components: {
    HelloWorld,
    VApp,
    VAppBar, 
    VToolbarTitle, 
    VContent 
  },
  data: () => ({
    //
  }),
};
</script>
