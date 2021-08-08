<template>
<div class="mt-5">
  <input placeholder="weight" type="number" class="text-align-center form-control" v-model="weightInput">
  <input class="form-control " v-if="startingWeightSet === false" type="button" @click="setWeight" value="Set the first weight">
  <input class="form-control" v-if="startingWeightSet === true" type="button" @click="weightCalculation" value="Calculate weight difference">
  <input type="button" class="form-control" @click="resetAll" value="Reset">

  <div>
    <div class="mt-5">
      <h1 v-if="lastWeight">Last weight {{lastWeight}}</h1>
      <h1 v-if="weightInput">Current weight {{weightInput}}</h1>
      <div v-if="weightStatus === 0 && startingWeightSet === true && lastWeight !== 0" class="mt-5">
        <p>You have lost </p>
        <p id="weightStatus0">{{weightDifferenceFixed}} kg</p>
        <iframe src="https://giphy.com/embed/d2Z9QYzA2aidiWn6" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/snl-thumbs-up-awesome-d2Z9QYzA2aidiWn6"></a></p>    </div>
    </div>
    <div v-if="weightStatus === 1" class="mt-5">
      <p>Weight has not changed and is still</p>
      <p id="weightStatus1">{{weightDifferenceFixed}} kg</p>
      <iframe src="https://giphy.com/embed/opzWPcGA4zRVQJxHzG" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/kimsconvenience-opzWPcGA4zRVQJxHzG"></a></p>
    </div>
    <div v-if="weightStatus === 2" class="mt-5">
      <p>You have gained</p>
      <p id="weightStatus2">{{weightDifferenceFixed}} kg</p>
      <div><iframe allow="fullscreen" frameBorder="0" height="270" src="https://giphy.com/embed/xSGJkRpfh7bPtdBkfj/video"></iframe></div>
    </div>
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
    },
    weightDifferenceFixed(){
      if (this.weightDifference > 0){
        return this.weightDifference.toFixed(2);
      }
      else {
        return this.weightDifference;
      }
    },
    resetAll(){
      localStorage.removeItem('lastWeight');
      window.location.reload();
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
      if (this.weightInput > 0){
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
      }else if (this.weightInput <= 0) {
        alert('Please use a value above 0')
      }

      this.$forceUpdate;
    },
    setWeight(){
      if (this.weightInput > 0){
        localStorage.setItem('lastWeight',this.weightInput);
        let lstWeight = localStorage.getItem('lastWeight');
        this.lastWeight = lstWeight;
        this.$forceUpdate;
        this.startingWeightSet = true;
      }
    }
  },
}
</script>

<style scoped>
input[type=button]{
  background: #2fb5ee;
  color: white;
}

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