 <template>
  <v-app>
    <v-app-bar app>
      <!-- -->
      <Toolbar />
    </v-app-bar>

    <!-- Sizes your content based upon application components -->
    <v-main>
      <!-- Provides the application the proper gutter -->
      <v-container fluid>
        <Main />
        <Places />
      </v-container>
    </v-main>

    <v-footer app>
      <!-- -->
    </v-footer>
  </v-app>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Places from "./components/Places.vue";
import Toolbar from "./components/Toolbar.vue";
import Main from "./components/Main.vue";

export default {
  name: "App",
  components: {
    Places,
    Toolbar,
    Main,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
