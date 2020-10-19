<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      <ul>
        <li v-for="error in errors">{{ error }}</li>
      </ul>
      
      Name: <input type="text" v-model="newPlaceName"><br>
      Address: <input type="text" v-model="newPlaceAddress"><br>
  
      <button v-on:click="createPlace()">Create Place</button>
    </div>

    <div v-for="place in places">
      <h2>Name: {{ place.name }}</h2>
      <p>Address: {{ place.address }}</p>
      <button v-on:click="showPlace(place)">More Info</button>
    </div>

    <dialog id="place-details">
      <form method="dialog">
        <h2>Place Info</h2>
        {{ currentPlace }}
        <p>Name: <input type="text" v-model="currentPlace.name"></p>
        <p>Address: <input type="text" v-model="currentPlace.address"></p>
        <button v-on:click="updatePlace(currentPlace)">Update Place</button>
        <button v-on:click="destroyPlace(currentPlace)">Destroy Place</button>
        <button>Close</button>
      </form>
    </dialog>

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Add New Place",
      places: [],
      newPlaceName: "",
      newPlaceAddress: "",
      currentPlace: {},
      errors: []
    };
  },
  created: function() {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function (){
      axios.get("/api/places").then(response => {
        console.log(response.data);
        this.places = response.data;
      });
    },
    createPlace: function () {
      var params = {
        name: this.newPlaceName,
        address: this.newPlaceAddress
      };
      axios.post("/api/places", params).then(response => {
        console.log("Success", response.data);
        this.places.push(response.data);
        this.newPlaceName = "";
        this.newPlaceAddress = "";
      })
      .catch(error => {
        console.log(error.response.data.errors);
        this.errors = error.response.data.errors;
      });
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = Place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      var params = {
        name: place.name,
        address: place.address,
      };
      axios.patch(`/api/places/${place.id}`, params).then(response => {
        console.log("Success", response.data);
      }).catch(error => {
        console.log(error.response.data.errors);
      });
    },
       destroyPlacee: function (place) {
      axios.delete(`/api/placess/${place.id}`).then(response => {
        console.log("Success", response.data);
        var index = this.places.indexOf(placee);
        this.places.splice(index, 1);
      });
    }
  }
};
</script>

