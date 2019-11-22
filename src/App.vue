<template>
  <div id="app">
    <div class="row center-align">
      <div class="col s12">
        <img alt="Vue logo" src="./assets/logo.png" />
      </div>
    </div>

    <CardPlate :cards="cards" @flip-card="flipCard"></CardPlate>

    <button @click="shuffleArray">Randomize</button>
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
          isFaceUp: false
        },
        {
          id: 1,
          name: "sun",
          isFaceUp: false
        },
        {
          id: 2,
          name: "sun",
          isFaceUp: false
        },
        {
          id: 3,
          name: "leaf",
          isFaceUp: false
        },
        {
          id: 4,
          name: "moon",
          isFaceUp: false
        },
        {
          id: 5,
          name: "star",
          isFaceUp: false
        },
        {
          id: 6,
          name: "leaf",
          isFaceUp: false
        },
        {
          id: 7,
          name: "moon",
          isFaceUp: false
        }
      ]
    };
  },
  methods: {
    shuffleArray() {
      this.cards.forEach(card => {
        card.isFaceUp = false;
      });

      this.cards.sort(function() {
        return 0.5 - Math.random();
      });
    },
    flipCard(id) {
      const card = this.cards.find(card => card.id === id);
      card.isFaceUp = !card.isFaceUp;
    }
  }
};
</script>
