<template lang="">
  <div 
  class="screen" 
  :style="{
  }">
    <div class="screen__inner">
      <card-flip
      v-for="(card, index) in cardsContext"
      :key="index"
      :ref="`card-${index}`"
      :imgBackFaceUrl="`images/${card}.png`"
      :card="{ index: index, value: card }"
      @onFlip="checkRule($event)"
      :numberCards="cardsContext.length"
    />
    </div>
  </div>
</template>
<script>
import CardItem from "./CardItem.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip: CardItem,
  },
  data() {
    return {
      rules: [],
      numbersCardFound: 0,
    };
  },
  methods: {
    checkRule(card) {

      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        // add class to card item
        this.$refs[`card-${this.rules[0].index}`][0].onDisableFlipCard();
        this.$refs[`card-${this.rules[1].index}`][0].onDisableFlipCard();

        // empty rules
        this.rules = [];
        this.numbersCardFound += 2;
        // check finished game

        if (this.numbersCardFound === this.cardsContext.length) {
          // finished game
          console.log("finished");
          setTimeout(() => {
            
              this.$emit("onFinished")
          }, 600);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {

        setTimeout(() => {
          for (let i in this.rules) {
            this.$refs[`card-${this.rules[i].index}`][0].onFlipBackCard();
          }
          this.rules = [];
        }, 500);
      } 
      else {
        return false
      }
    },
  },
};
</script>
<style lang="scss" scoped>
  .screen {
    .screen__inner {
      height: var(--height-body);
      width: calc(var(--height-body) * 3/4);
      margin: auto;
    }
  }
</style>
