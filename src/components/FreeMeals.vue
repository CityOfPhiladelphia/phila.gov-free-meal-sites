<template>
  <div>
    <h2>FREE MEAL SITES</h2>
    <div class="search">
    <input
        id="search-bar"
        v-model="search"
        class="search-field"
        type="text"
        placeholder="Search by title or keyword"
      ><input
        ref="archive-search-bar"
        type="submit"
        class="search-submit"
        value="Search"
      >
      <button
        v-if="search.length > 0"
        class="clear-search-btn"
        @click="clearSearchBar"
      >
        <i class="fas fa-times " />
      </button>
    </div>
    <table>
      <thead>
        <tr>
          <td>Name</td>
          <td>Address</td>
        </tr>
      </thead>
      <tbody>
      <tr v-for="site in filteredSites" 
          :key="site.ObjectID">
        
          <td>{{site.Agency_Name }}</td>
         <td>{{ site.Address }} {{ site.Zip_Code}}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
import VueFuse from "vue-fuse";
import Vue from 'vue';


const endpoint = "https://services.arcgis.com/fLeGjb7u4uXqeF9q/arcgis/rest/services/FILE_Community_Food_Hubs/FeatureServer/0/query?where=1%3D1&&outFields=*&f=pjson";
Vue.use(VueFuse);

export default {
  name: 'FreeMeals',
  data: function() {
    return {
      mealSites: [],
      filteredSites: [],
      search: "",
      searchOptions: {
        shouldSort: true,
        threshold: 0.1,
        tokenize: true,
        keys: [
          'Agency_Name',
          'Address',
          'Zip_Code'
        ],
      },
    }
  },
  mounted() {
    this.fetchData()
  },
  watch: {
    search() {
      this.searchTable();
    },
  },
  methods: {
    fetchData: function() {
      axios.get(endpoint)
      .then((response) => {
        response.data.features.forEach((item)=> {
          this.mealSites.push(item.attributes);
          this.filteredSites.push(item.attributes)
        })
      })
      .catch((e) => {
        console.log(e)
      })
    },

    clearSearchBar: function() {
      this.search = "";
    },

    searchTable: function() {
     
      if (this.search) {
        //  let tempSites = [];
        this.$search(this.search, this.mealSites, this.searchOptions).then(sites => {
          this.filteredSites = sites;
        });
      } else {

        this.filteredSites = this.mealSites;
      }
    }
  },
}
</script>

<style scoped>
.clear-search-btn {
      position: absolute;
      top:16px;
      right: 70px;
      padding: 0;
      font-size: 20px;
      background-color: #fff;
      opacity: 0.8;
      z-index: 999;
      cursor: pointer;
      color: rgba(60, 60, 60, 0.5);
      
    }
</style>
