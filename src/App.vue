<template>
  <div id="app">
    <div class="row center-align">
      <div class="col s12">
        <img alt="Vue logo" src="./assets/logo.png" />
      </div>
    </div>
    {{cardsCanBeFlipped}}
    <CardPlate :cards="cards"></CardPlate>

    <button class="btn waves waves-light" @click="flipCardsFaceDown">Randomize</button>
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
          isFaceUp: true,
          isTransitioning: false,
          canBeFlipped: false
        },
        {
          id: 1,
          name: "sun",
          isFaceUp: true,
          isTransitioning: false,
          canBeFlipped: false
        },
        {
          id: 2,
          name: "sun",
          isFaceUp: true,
          isTransitioning: false,
          canBeFlipped: false
        },
        {
          id: 3,
          name: "leaf",
          isFaceUp: true,
          isTransitioning: false,
          canBeFlipped: false
        },
        {
          id: 4,
          name: "moon",
          isFaceUp: true,
          isTransitioning: false,
          canBeFlipped: false
        },
        {
          id: 5,
          name: "star",
          isFaceUp: true,
          isTransitioning: false,
          canBeFlipped: false
        },
        {
          id: 6,
          name: "leaf",
          isFaceUp: true,
          isTransitioning: false,
          canBeFlipped: false
        },
        {
          id: 7,
          name: "moon",
          isFaceUp: true,
          isTransitioning: false,
          canBeFlipped: false
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

      setTimeout(this.shuffleCards, 1000);
    },
    shuffleCards() {
      this.cards.sort(function() {
        return 0.5 - Math.random();
      });

      this.cards.forEach(card => {
        card.canBeFlipped = true;
      });
    },
    selectCard(card) {
      if (this.selectedCards.length < 2 && card.canBeFlipped) {
        const cardIndex = this.selectedCards.indexOf(card);
        card.isFaceUp = !card.isFaceUp;

        if (cardIndex === -1) {
          this.selectedCards.push(card);
        } else {
          this.selectedCards.splice(cardIndex, 1);
        }
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
        });
      }
      this.selectedCards.length = 0;
    }
  },
  mounted() {
    EventBus.$on("card-transition-start", card => {
      if (this.cardsCanBeFlipped) {
        console.log(`Card #${card.id} transition started.`);
        card.canBeFlipped = false;
      }
    });
    EventBus.$on("card-transition-end", card => {
      if (this.cardsCanBeFlipped) {
        console.log(`Card #${card.id} transition ended.`);

        if (this.selectedCards.length === 2) {
          console.log("2 Cards selected, executing card check.");
          this.checkSelectedCards();
        } else {
          card.canBeFlipped = true;
        }
      }
    });
    EventBus.$on("flip-card", card => {
      this.selectCard(card);
    });
  },
  computed: {
    cardsCanBeFlipped() {
      return this.cards.some(card => card.canBeFlipped === true);
    }
  }
};
</script>
