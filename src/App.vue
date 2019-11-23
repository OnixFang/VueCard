<template>
  <div id="app">
    <div class="row center-align">
      <div class="col s12">
        <img alt="Vue logo" src="./assets/logo.png" />
      </div>
    </div>

    <CardPlate :cards="cards" @flip-card="flipCard"></CardPlate>

    <button @click="flipCardsFaceDown">Randomize</button>
  </div>
</template>

<script>
import CardPlate from "./components/CardPlate.vue";

export default {
  name: "app",
  components: {
    CardPlate
  },
  data() {
    return {
      cards: [
        {
          id: 0,
          name: "star",
          isFaceUp: false,
          canBeFlipped: true
        },
        {
          id: 1,
          name: "sun",
          isFaceUp: false,
          canBeFlipped: true
        },
        {
          id: 2,
          name: "sun",
          isFaceUp: false,
          canBeFlipped: true
        },
        {
          id: 3,
          name: "leaf",
          isFaceUp: false,
          canBeFlipped: true
        },
        {
          id: 4,
          name: "moon",
          isFaceUp: false,
          canBeFlipped: true
        },
        {
          id: 5,
          name: "star",
          isFaceUp: false,
          canBeFlipped: true
        },
        {
          id: 6,
          name: "leaf",
          isFaceUp: false,
          canBeFlipped: true
        },
        {
          id: 7,
          name: "moon",
          isFaceUp: false,
          canBeFlipped: true
        }
      ].sort(function() {
        return 0.5 - Math.random();
      })
    };
  },
  methods: {
    flipCardsFaceDown() {
      this.cards.forEach(card => {
        card.isFaceUp = false;
      });

      setTimeout(this.shuffleArray, 1000);
    },
    shuffleArray() {
      this.cards.sort(function() {
        return 0.5 - Math.random();
      });

      this.cards.forEach(card => {
        card.canBeFlipped = true;
      });
    },
    flipCard(id) {
      const card = this.cards.find(card => card.id === id);
      if (card.canBeFlipped) {
        card.isFaceUp = !card.isFaceUp;
        card.canBeFlipped = false;
      }
    }
  }
};
</script>
