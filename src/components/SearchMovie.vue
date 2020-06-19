<template>
  <v-card color="indigo lighten-2" dark>
    <v-card-text>
      <v-autocomplete
        v-model="model"
        :items="items"
        :loading="isLoading"
        :search-input.sync="search"
        color="white"
        hide-no-data
        hide-selected
        item-text="Description"
        item-value="API"
        placeholder="Start typing to Search"
        prepend-icon="mdi-database-search"
        return-object
      ></v-autocomplete>
    </v-card-text>
    <v-divider></v-divider>
    <v-expand-transition>
      <v-list v-if="model">
        <v-list-item>
            <v-img :src="model.Poster" aspect-ratio="2" contain></v-img>
        </v-list-item>
        <v-list-item v-for="(field, i) in fields" :key="i">
          <v-list-item-content>
            <v-list-item-title v-text="field.value"></v-list-item-title>
            <v-list-item-subtitle v-text="field.key"></v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-expand-transition>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn :disabled="!model" color="red darken-3" @click="model = null">
        Clear
        <v-icon right>mdi-close-circle</v-icon>
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import axios from "axios";
export default {
  name: "SearchMovie",
  data: () => ({
    descriptionLimit: 50,
    entries: [],
    isLoading: false,
    model: null,
    search: null,
    timeOut: null
  }),
  computed: {
    fields() {
      if (!this.model) return [];

      return Object.keys(this.model).map(key => {
        return {
          key,
          value: this.model[key] || "n/a"
        };
      });
    },
    items() {
      return (
        this.entries &&
        this.entries.map(entry => {
          const Description =
            entry.Title.length > this.descriptionLimit
              ? entry.Title.slice(0, this.descriptionLimit) + "..."
              : entry.Title;

          return Object.assign({}, entry, { Description });
        })
      );
    }
  },

  watch: {
    search(val) {
      this.isLoading = true;
      if (this.timeOut) {
        clearInterval(this.timeOut);
      }
      this.timeOut = setTimeout(() => {
        //fetch Movies
        if (val === null) return;
        axios
          .get("http://www.omdbapi.com/", {
            params: {
              apikey: "3feec314",
              s: val
            }
          })
          .then(res => {
            this.entries = res.data.Search;
          })
          .catch(err => {
            console.log(err);
          })
          .finally(() => (this.isLoading = false));
      }, 500);
      this.isLoading = false;
    }
  }
};
</script>

<style>
</style>