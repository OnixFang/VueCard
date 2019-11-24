<template>
  <div id="app">
    <div class="row center-align">
      <div class="col s12">
        <img alt="Vue logo" src="./assets/logo.png" />
      </div>
    </div>
    {{gameStarted}}
    <CardPlate :cards="cards"></CardPlate>

    <button class="btn waves waves-light" @click="flipCardsFaceDown">Shuffle Cards</button>
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
          canBeFlipped: false
        },
        {
          id: 1,
          name: "sun",
          isFaceUp: true,
          canBeFlipped: false
        },
        {
          id: 2,
          name: "sun",
          isFaceUp: true,
          canBeFlipped: false
        },
        {
          id: 3,
          name: "leaf",
          isFaceUp: true,
          canBeFlipped: false
        },
        {
          id: 4,
          name: "moon",
          isFaceUp: true,
          canBeFlipped: false
        },
        {
          id: 5,
          name: "star",
          isFaceUp: true,
          canBeFlipped: false
        },
        {
          id: 6,
          name: "leaf",
          isFaceUp: true,
          canBeFlipped: false
        },
        {
          id: 7,
          name: "moon",
          isFaceUp: true,
          canBeFlipped: false
        }
      ].sort(function() {
        return 0.5 - Math.random();
      }),
      selectedCards: [],
      playerCanFlip: false,
      gameStarted: false
    };
  },
  methods: {
    flipCardsFaceDown() {
      this.gameStarted = false;
      this.playerCanFlip = false;

      this.cards.forEach(card => {
        card.isFaceUp = false;
      });

      setTimeout(this.shuffleCards, 1500);
    },
    shuffleCards() {
      this.cards.sort(function() {
        return 0.5 - Math.random();
      });

      this.cards.forEach(card => {
        card.canBeFlipped = true;
      });

      M.toast({
        html: "All cards shuffled!",
        classes: "rounded green darken-3",
        displayLength: 1000
      });

      this.gameStarted = true;
      this.playerCanFlip = true;
    },
    selectCard(card) {
      if (this.selectedCards.length < 2) {
        if (card.canBeFlipped) {
          card.isFaceUp = !card.isFaceUp;
        } else {
          M.toast({
            html: "This card cannot be flipped.",
            classes: "rounded green darken-3",
            displayLength: 2000
          });
          this.playerCanFlip = true;
        }
      } else {
        M.toast({
          html: "Wait for the cards to be compared!",
          classes: "rounded green darken-3",
          displayLength: 2000
        });
      }
    },
    compareCards() {
      const card1 = this.selectedCards[0];
      const card2 = this.selectedCards[1];

      if (card1.name === card2.name) {
        this.selectedCards.forEach(card => {
          card.canBeFlipped = false;
        });

        M.toast({
          html: "The cards match!",
          classes: "rounded blue darken-3",
          displayLength: 1000
        });
      } else {
        this.selectedCards.forEach(card => {
          card.canBeFlipped = true;
          card.isFaceUp = false;
        });

        M.toast({
          html: "The cards do not match!",
          classes: "rounded red darken-3",
          displayLength: 1000
        });
      }
      this.selectedCards = [];
    }
  },
  mounted() {
    EventBus.$on("card-transition-start", card => {
      if (this.gameStarted) {
        console.log(`Card #${card.id} transition started.`);
        card.canBeFlipped = false;
      }
    });
    EventBus.$on("card-transition-end", card => {
      if (this.gameStarted) {
        console.log(`Card #${card.id} transition ended.`);

        const cardIndex = this.selectedCards.indexOf(card);

        // check if card is already selected
        if (cardIndex === -1) {
          // add card
          this.selectedCards.push(card);

          // check if two cards have been selected
          if (this.selectedCards.length === 2) {
            console.log("2 Cards selected, executing card check.");
            this.compareCards();
          }
        } else {
          // remove card
          this.selectedCards.splice(cardIndex, 1);
          card.canBeFlipped = true;
        }

        // enable player movement
        this.playerCanFlip = true;
      }
    });
    EventBus.$on("flip-card", card => {
      if (!this.gameStarted) {
        M.toast({
          html: "Shuffle the cards to start!",
          classes: "rounded green darken-3",
          displayLength: 1000
        });
      } else {
        if (this.playerCanFlip) {
          this.playerCanFlip = false;
          this.selectCard(card);
        } else {
          M.toast({
            html: "Wait for the game to process your move.",
            classes: "rounded green darken-3",
            displayLength: 2000
          });
        }
      }
    });
  }
};
</script>
