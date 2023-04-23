<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen
   v-if="statusMatch === 'match'" 
   :cardsContext="settings.cardsContext"
   @onFinished="onGetResult"
   />
   <result-screen 
   v-if="statusMatch ==='result'"
   :timer="timer"
   @onStartAgain="onStartAgain"
    />
    <p class="copyright">
    This game owned by RHP Team in Vue 3 course for begginers -
    <a
      href="https://www.youtube.com/watch?v=CHM75-NqOmk&list=PLU4OBh9yHE94sZ3TPGt0QG_PIwrZ1QF6i"
      >view here</a
    >
  </p>
</template>

<script>
import MainScreen from './components/MainScreen.vue';
import InteractScreen from './components/InteractScreen.vue';
import ResultScreen from './components/ResultScreen.vue';

import {shuffleArray} from "@/utils/array"
export default {
  name: "App",
  components: { MainScreen,InteractScreen,ResultScreen },
  created() {
    const width = window.innerWidth;
const height = window.innerHeight;

console.log(`The viewport's width is ${width} and the height is ${height}.`);
  },
  data() {
    return {
      settings:{
        totalOfBlocks:0,
        cardsContext:[],
        startedAt:null
      },
      statusMatch:"default",
      timer:0  
    }
  },
  methods: {
    onHandleBeforeStart(config){
      // console.log("running handle before start: ", config)
      this.settings.totalOfBlocks = config?.totalOfBlocks ?? 0;
      const firstCards = Array.from({length:this.settings.totalOfBlocks/2},(_,i)=>i+1)
      const secondCards = [...firstCards];
      const cards = [...firstCards,...secondCards]
      
      this.settings.cardsContext = shuffleArray(shuffleArray(shuffleArray(cards)))
      // console.log(this.settings.cardsContext)

      this.settings.startedAt = new Date().getTime();
      
      
      // data ready
      this.statusMatch = "match"
      console.log(this.settings.cardsContext)
    },
    onGetResult(){
      // get timer
      this.timer = new Date().getTime() - this.settings.startedAt;

      // switch result screen
      this.statusMatch = "result";
    },
    onStartAgain(){
      this.statusMatch = "default"
    }
  },
};
</script>
<style lang="scss">
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}
.copyright a {
  color: #f4dc26;
}
</style>
