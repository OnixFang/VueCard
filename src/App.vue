<template>
  <div id="app">
    <div class="row center-align">
      <div class="col s12">
        <img alt="Vue logo" src="./assets/logo.png" />
      </div>
    </div>

    <CardPlate :cards="cards"></CardPlate>

    <button @click="flipCardsFaceDown">Randomize</button>
  </div>
</template>

<script>
import CardPlate from "./components/CardPlate";
import * as M from "materialize-css";
import EventBus from "./EventBus";

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
          isTransitioning: false,
          canBeFlipped: true
        },
        {
          id: 1,
          name: "sun",
          isFaceUp: false,
          isTransitioning: false,
          canBeFlipped: true
        },
        {
          id: 2,
          name: "sun",
          isFaceUp: false,
          isTransitioning: false,
          canBeFlipped: true
        },
        {
          id: 3,
          name: "leaf",
          isFaceUp: false,
          isTransitioning: false,
          canBeFlipped: true
        },
        {
          id: 4,
          name: "moon",
          isFaceUp: false,
          isTransitioning: false,
          canBeFlipped: true
        },
        {
          id: 5,
          name: "star",
          isFaceUp: false,
          isTransitioning: false,
          canBeFlipped: true
        },
        {
          id: 6,
          name: "leaf",
          isFaceUp: false,
          isTransitioning: false,
          canBeFlipped: true
        },
        {
          id: 7,
          name: "moon",
          isFaceUp: false,
          isTransitioning: false,
          canBeFlipped: true
        }
      ].sort(function() {
        return 0.5 - Math.random();
      }),
      selectedCards: []
    };
  },
  methods: {
    flipCardsFaceDown() {
      M.toast({
        html: "All cards shuffled!",
        classes: "rounded green darken-3",
        displayLength: 1000
      });

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
    selectCard(card) {
      if (this.selectedCards.length < 2 && card.canBeFlipped) {
        card.canBeFlipped = false;
        this.selectedCards.push(card);
        card.isFaceUp = !card.isFaceUp;
      } else {
        M.toast({
          html: "Wait for the cards to be compared!",
          classes: "rounded green darken-3",
          displayLength: 2000
        });
      }
    },
    checkSelectedCards() {
      if (this.selectedCards[0].name === this.selectedCards[1].name) {
        M.toast({
          html: "The cards match!",
          classes: "rounded blue darken-3",
          displayLength: 1000
        });
      } else {
        M.toast({
          html: "The cards do not match!",
          classes: "rounded red darken-3",
          displayLength: 1000
        });
        this.selectedCards.forEach(card => {
          card.isFaceUp = false;
          card.canBeFlipped = true;
        });
      }
      this.selectedCards.length = 0;
    }
  },
  mounted() {
    EventBus.$on("card-transition-start", card => {
      console.log(`Card #${card.id} transition started.`);
    });
    EventBus.$on("card-transition-end", card => {
      console.log(`Card #${card.id} transition ended.`);
      if (this.selectedCards.length === 2) {
        console.log("2 Cards selected, executing card check.");
        this.checkSelectedCards();
      }
    });
    EventBus.$on("flip-card", card => {
      this.selectCard(card);
    });
  }
};
</script>
