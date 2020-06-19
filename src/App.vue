<template>
  <v-app>
    <Navbar />
    <v-main>
      <v-container>
    <v-row>
      <v-col cols="12"
        md="6">
       <FirstMovie />
      </v-col>
      <v-col cols="12"
        md="6">
       <SecondMovie />
      </v-col>
    </v-row>
  </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Navbar from "@/components/Navbar"
import FirstMovie from "@/components/FirstMovie"
import SecondMovie from "@/components/SecondMovie"
import axios from"axios"
export default {
  name: "App",
  components: {
    Navbar,
    FirstMovie,
    SecondMovie
  },
  methods: {
    fetchMovie: async () => {
      try {
        const res = await axios.get('http://www.omdbapi.com/',{
          params:{
            apikey:'3feec314',
            s:'avengers'
          }
        });
        console.log(res.data);
        
      } catch (error) {
        console.log(error);
      }
    }
  },
  created() {
    // this.fetchMovie();
  }
}
</script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>