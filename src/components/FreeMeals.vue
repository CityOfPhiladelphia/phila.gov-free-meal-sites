<template>
  <div>
    <h2>FREE MEAL SITES</h2>
    <table>
      <thead>
        <tr>
          <td>Name</td>
          <td>Address</td>
        </tr>
      </thead>
      
      <tbody>
      <tr v-for="site in mealSites" 
          :key="site.attributes.ObjectID">
        
          <td>{{site.attributes.Agency_Name }}</td>
         <td>{{ site.attributes.Address }} {{ site.attributes.Zip_Code}}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";

const endpoint = "https://services.arcgis.com/fLeGjb7u4uXqeF9q/arcgis/rest/services/FILE_Community_Food_Hubs/FeatureServer/0/query?where=1%3D1&&outFields=*&f=pjson";

export default {
  name: 'FreeMeals',
  data: function() {
    return {
      mealSites: [],
    }
  },
  mounted() {
    this.fetchData()
  },
  methods: {
    fetchData: function() {
      axios.get(endpoint)
      .then((response) => {
        this.mealSites = response.data.features;
      })
      .catch((e) => {
        console.log(e)
      })
    }
  },
}
</script>

<style scoped>

</style>
