<template>
<div class="mt-5">
  <input placeholder="weight" class="text-align-center" v-model="weightInput">
  <input class="form-control" v-if="startingWeightSet === true" type="button" @click="weightCalculation" value="Calculate weight difference">
  <input class="form-control" v-if="startingWeightSet === false" type="button" @click="setWeight" value="Set the first weight">



  <div class="mt-5">
    <h1 v-if="lastWeight">Last weight {{lastWeight}}</h1>
    <h1 v-if="weightInput">Current weight {{weightInput}}</h1>

    <div v-if="weightStatus === 0 && startingWeightSet === true && lastWeight !== 0" class="mt-5">
      <p>You have lost </p>
      <p id="weightStatus0">{{weightDifference.toFixed(2)}} kg</p>
      <iframe src="https://giphy.com/embed/d2Z9QYzA2aidiWn6" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/snl-thumbs-up-awesome-d2Z9QYzA2aidiWn6"></a></p>    </div>

  </div>

    <div v-if="weightStatus === 1" class="mt-5">
      <p>Weight has not changed and is still</p>
      <p id="weightStatus1">{{weightDifference.toFixed(2)}} kg</p>
      <iframe src="https://giphy.com/embed/opzWPcGA4zRVQJxHzG" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/kimsconvenience-opzWPcGA4zRVQJxHzG"></a></p>
    </div>


    <div v-if="weightStatus === 2" class="mt-5">
      <p>You have gained</p>
      <p id="weightStatus2">{{weightDifference.toFixed(2)}} kg</p>
      <iframe allow="fullscreen" frameBorder="0" height="480" src="https://giphy.com/embed/CvF6vRAf7QUgCvQ9KK/video"></iframe>
    </div>
</div>
</template>

<script>
export default {
  name: "WeightTracking",
  data(){
    return {
      weightInput : 0,
      currentWeight : 0,
      weightDifference : 0,
      startingWeightSet : false,
      lastWeight : 0,
      weightStatus : 0
    }
  },
  computed : {
    retrieveLstWeight(){
      return localStorage.getItem('lastWeight');
    }
  },
  mounted(){
    //TODO: Check if localStorage weight has a value
    if (localStorage.getItem('lastWeight')){
      this.startingWeightSet = true;
    }
    else {
      this.startingWeightSet = false;
    }
  },
  methods: {
    //TODO: Optional (Give the option to display in kg or grams)


    //WEIGHT STATUS 0 = LOST WEIGHT
    //WEIGHT STATUS 1 = SAME WEIGHT
    //WEIGHT STATUS 2 = GAINED WEIGHT
    weightCalculation(){
      let lastWeight = localStorage.getItem('lastWeight');
      this.lastWeight = lastWeight;
      if (lastWeight){
        if (lastWeight > this.weightInput){
          this.weightDifference = lastWeight - this.weightInput;
          this.weightStatus = 0;
        }
        else if (lastWeight === this.weightInput){
          this.weightDifference = this.weightInput;
          this.weightStatus = 1;
        }
        else if (lastWeight < this.weightInput) {
          this.weightDifference = this.weightInput - lastWeight ;
          this.weightStatus = 2;
        }
        else {
          this.weightStatus = null
        }

        localStorage.setItem('lastWeight',this.weightInput);
      }
      else {
        localStorage.setItem('lastWeight',this.weightInput);
      }

      this.$forceUpdate;
    },
    setWeight(){
      localStorage.setItem('lastWeight',this.weightInput);
      let lstWeight = localStorage.getItem('lastWeight');
      this.lastWeight = lstWeight;
      this.$forceUpdate;
      this.startingWeightSet = true;
    }
  },
}
</script>

<style scoped>
#weightStatus0{
  color: green;
}

#weightStatus1 {
  color: black;
}

#weightStatus2 {
  color: red;
}

.mt-5 {
  font-size: 20px;
}

.form-control {
  margin: auto !important;
  width: 68% !important;
}
</style>
<style>
.text-align-center {
  text-align: center;
}
</style>