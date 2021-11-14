<template>
  <div class="welcome">
    <h1>welcome</h1>
    <h2><b>reaction time game</b></h2>
    <h4>
      click the
      <span style="color: green; font-family: cursive"> green</span>
      shapes
      <u> only!</u>
    </h4>
  </div>
  <div v-show="!showShape">
    <button class="buttonStyle" @click="randomShapeAndColor">
      when you are ready
    </button>
  </div>
  <div v-if="showShape">
    <Shape @close="stopShape" :shape="currentShape" :color="currentColor" />
  </div>
  <div v-if="showIndicator">
    <h1>good job</h1>
  </div>
  <div v-if="showResults">
    <Endgame
      :score="greenCounter"
      :time="timeplayed"
      :scoreInWords="successIndicator"
      :highestScore="highestScore"
      @closeEndGame="closeEndGame"
    />
  </div>
</template>

<script>
import Endgame from "./components/Endgame";
import Shape from "./components/Shape.vue";

export default {
  name: "App",
  data() {
    return {
      showShape: false,
      timerDelay: null,
      printScore: false,
      colorArray: ["yellow", "green", "red", "blue"],
      shapeArray: ["circle", "rectangle", "triangular"],
      currentShape: null, //will hold one shape at a time from ShapeArray
      currentColor: null, //will hold one color at a time from colorArray
      clickOnGreenFlag: false,
      showResults: false, // show the end game screen
      intervalShape: null,
      greenCounter: 0, //how many time to player succed
      timer: null,
      timeplayed: 0,
      showIndicator: false,
      showIndicatorInterval: null,
      successIndicator: null, //will chane to sentence according to 'greenCounter'
      highestScore: 0,
    };
  },
  components: { Shape, Endgame },
  methods: {
    startTimer() {
      this.timer = setInterval(() => {
        this.timeplayed += 10;
      }, 10);
    },
    stopTimer() {
      clearInterval(this.timer);
    },
    //
    randomShapeAndColor() {
      // this.timerDelay = 300 + Math.random() * 400;
      this.startTimer();
      this.intervalShape = setInterval(this.addShape, 500);
    },
    addShape() {
      this.currentColor = this.colorArray[Math.floor(Math.random() * 4)];
      this.currentShape = this.shapeArray[Math.floor(Math.random() * 3)];

      this.showResults = false;
      this.showShape = true;
      // this.toggleShowShape();
    },
    toggleShowShape() {
      this.showShape = !this.showShape;
    },
    stopShape(clickOnGreen) {
      this.clickOnGreenFlag = clickOnGreen;
      if (this.clickOnGreenFlag) {
        this.showIndicator = true;
        this.showIndicatorInterval = setInterval(this.closeIndicator, 500);
        this.greenCounter++;
      } else {
        clearInterval(this.intervalShape);
        this.stopTimer();
        this.successIndicator = this.giveSuccessIndicator(); //getting value for seccess
        this.timeplayed = this.timeplayed / 1000; //from ms to seconds
        this.timeplayed = parseFloat(this.timeplayed.toFixed(4)); //only 4 digit decimal
        this.showShape = false; //stop showing the shapes on the DOM
        if (this.greenCounter > this.highestScore) {
          //need to upload name and score
          this.highestScore = this.greenCounter;
        }
        this.showResults = true; //showing the end game DOM
      }
    },
    closeIndicator() {
      clearInterval(this.showIndicatorInterval);
      this.showIndicator = false;
    },
    closeEndGame() {
      this.showResults = false;
      this.greenCounter = 0;
      this.endTimer = 0;
    },
    giveSuccessIndicator() {
      //returing sentnce according to the value
      if (this.greenCounter <= 3) return "You snooze you lose";
      else if (this.greenCounter <= 6) return "Congrats on a mediocre job";
      else return "Eternal bragging rights";
    },
  },
};
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.welcome h1 {
  color: blueviolet;
}
</style>
