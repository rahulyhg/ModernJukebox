<template>
  <div id="chooserContainer">
    <h3>Please select the venue you're in.</h3>
    <select id="chooser" v-model="selectedRestaurant">
      <option selected disabled value="">Choose venue</option>
      <option
        v-for="restaurant in restaurants"
        :value="restaurant">
        {{restaurant.name}}
      </option>
    </select>
  </div>
</template>

<script>
  import {db} from '../firebase';

  export default {
    name: 'restaurantChooser',
    data() {
      return {
        restaurants: [],
        selectedRestaurant: ''
      }
    },
    methods: {
      getRestaurants() {
        db.ref().on('value', snapshot => {
          snapshot.forEach(child => {
            this.restaurants.push(child.val())
          })
        })
      },
      setRestaurant() {
        if(this.selectedRestaurant) {
          this.$emit('setRestaurant', this.selectedRestaurant.id)
        }
      }
    },
    mounted() {
      this.getRestaurants()
    },
    updated() {
      this.setRestaurant()
    }
  }
</script>

<style>
  #chooserContainer{
    min-height: 100vh;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 1rem 0;
  }

  #chooserContainer h3{
    font-size: 1.3rem;
    font-weight: bold;
  }

  #chooser{
    appearance: none;

    margin-top: 2rem;

    font-size: var(--button);
    color: var(--textColorDark);
    font-family: 'Roboto Condensed', sans-serif;
    text-transform: uppercase;
    font-weight: bold;

    border: none;
    border-radius: 10rem;
    padding: 1rem 2rem;
    background-color: var(--mainColor);
    background-image:url("../assets/ic_arrow_drop_down.svg");
    background-position: right 1.5rem center;
    background-repeat: no-repeat;
    }
</style>
