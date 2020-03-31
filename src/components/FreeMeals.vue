<template>
  <div>
    <h2>Free meal sites</h2>
    <div class="search">
    <input
        id="search-bar"
        v-model="search"
        class="search-field"
        type="text"
        placeholder="Filter by site name or address"
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
    <div
      v-show="loading"
      class="mtm center"
    >
      <i class="fas fa-spinner fa-spin fa-3x" />
    </div>
    <div
      v-show="!loading && empty"
      class="h3 mtm center"
    >
      Sorry, there are no results.
    </div>
    <div
      v-show="failure"
      class="h3 mtm center"
    >
      Sorry, there was a problem. Please try again.
    </div>
    <table
      v-show="!empty && !loading && !failure"
    >
      <thead>
        <tr>
          <th
            class="table-sort title"
            :class="currentSortDir"
            @click="toggleSort()"
          >
            <span>Food site</span>
          </th>
          <th>Address (ZIP code) </th>
        </tr>
      </thead>
      <tbody>
      <tr v-for="site in filteredSites" 
          :key="site.ObjectID">
          <td>{{site.Agency_Name }}</td>
         <td>{{ site.Address }} ({{ site.Zip_Code}})</td>
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
      loading: false,
      failure: false,
      empty: false,
      currentSortDir: 'desc',
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
    filteredSites() {
      if (!this.failure) {
        if (this.filteredSites.length == 0) {
          this.empty = true;
        } else  {
          this.empty = false;
        }
      }
    }
  },

  methods: {
    fetchData: function() {
      this.loading = true;
      axios.get(endpoint)
      .then((response) => {
        response.data.features.forEach((item)=> {
          this.mealSites.push(item.attributes);
          this.filteredSites.push(item.attributes)
        })
        this.sortSites();
        this.failure = false;
        this.loading = false;
      })
      .catch((e) => {
        console.log(e)
        this.loading = false;
        this.failure = true;
      })
    },

    toggleSort: function() {
       this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
       this.sortSites()
    },

    sortSites:function() {
      this.filteredSites.sort((a, b) => {
         let modifier = -1;
          if(this.currentSortDir === 'desc') {
            modifier = 1;
          }
        var textA = a.Agency_Name.toUpperCase();
        var textB = b.Agency_Name.toUpperCase();
        return (textA < textB) ? ( -1 * modifier) : (textA > textB) ? (1 * modifier) : 0;
      });
    },

    clearSearchBar: function() {
      this.search = "";
    },

    searchTable: function() {
      if (this.search) {
        this.$search(this.search, this.mealSites, this.searchOptions).then(sites => {
          this.filteredSites = sites;
        });
      } else {
        this.filteredSites = this.mealSites;
        this.sortSites();
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
