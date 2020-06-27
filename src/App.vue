<template>
  <v-app>
    <Navbar />
    <v-main>
      <v-container fluid>
        <v-container>
          <v-row justify="center">
            <v-btn
              :disabled="disabled"
              color="red darken-2"
              class="white--text"
              @click="startFight"
            >
              Start Fight
              <v-icon right dark>sync</v-icon>
            </v-btn>
          </v-row>
        </v-container>

        <v-row justify="center">
          <v-col cols="12" red md="5" :class="leftMovieClass">
            <v-card outlined tile>
              <SearchMovie v-on:movie-selected="getLeftMovie" v-on:clear="clear" />
            </v-card>
          </v-col>
          <v-col md="1"></v-col>
          <v-col cols="12" md="5" class="right-movie" :class="rightMovieClass">
            <v-card outlined tile>
              <SearchMovie v-on:movie-selected="getRightMovie" v-on:clear="clear" />
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <div class="text-center">
      <v-dialog v-model="dialog" hide-overlay persistent width="300">
        <v-card color="primary" dark>
          <v-card-text class="pt-2">
            Fighting Movies.....
            <v-progress-linear indeterminate color="white" class="mt-2" height="8"></v-progress-linear>
          </v-card-text>
        </v-card>
      </v-dialog>
    </div>
      <footer class="bg-primary">
      <h5 class="text-center py-2 text-white"> &copy; 2020 Flamur Deliu</h5>
    </footer>
  </v-app>
</template>

<script>
import Navbar from "@/components/Navbar";
import SearchMovie from "@/components/SearchMovie";
export default {
  name: "App",
  components: {
    Navbar,
    SearchMovie
  },
  data() {
    return {
      leftMovie: null,
      rightMovie: null,
      fight: null,
      disabled: true,
      dialog: false
    };
  },
  methods: {
    getLeftMovie(movie) {
      this.leftMovie = movie;
      if (this.rightMovie) {
        this.disabled = false;
      }
    },
    getRightMovie(movie) {
      this.rightMovie = movie;
      if (this.leftMovie) {
        this.disabled = false;
      }
    },
    startFight() {
      this.dialog = true;
      setTimeout(() => {
        this.fight = true;
      }, 2000);
    },
    clear() {
      this.disabled = true;
      this.fight = false;
    }
  },
  computed: {
    leftMovieClass() {
      if (this.fight) {

        return parseFloat(this.leftMovie.imdbRating) >
          parseFloat(this.rightMovie.imdbRating)
          ? "green"
          : "yellow";
      } else {
        return "";
      }
    },
    rightMovieClass() {
      if (this.fight) {
        return parseFloat(this.leftMovie.imdbRating) <
          parseFloat(this.rightMovie.imdbRating)
          ? "green"
          : "yellow";
      } else {
        return "";
      }
    }
  },
  watch: {
    dialog(val) {
      if (!val) return;

      setTimeout(() => (this.dialog = false), 2000);
    }
  }
};
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background: url("./assets/background-image.jpg") no-repeat center center/cover;
}

footer{
  position: fixed;
  width: 100%;
  left:0;
  bottom: 0;
}
</style>