<template>
  <div class="col s2">
    <div class="card-container">
      <div class="card" :class="{ flip : cardObj.isFaceUp }" @click="flipCard">
        <figure>
          <img src="../assets/back-card.png" />
        </figure>
        <figure class="back">
          <img :src="cardImage" />
        </figure>
      </div>
    </div>
  </div>
</template>

<script>
import { log } from 'util';
export default {
  name: "Card",
  props: {
    cardObj: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      cardImage: require(`../assets/${this.cardObj.name}-card.png`)
    };
  },
  methods: {
    flipCard() {
      this.$emit("flip-card", this.cardObj.id);
    }
  }
};
</script>

<style scoped>
.card-container {
  width: 170px;
  height: 250px;
  position: relative;
  perspective: 800px;
}

.card {
  width: 100%;
  height: 100%;
  position: absolute;
  transform-style: preserve-3d;
  transition: transform 1s;
}

.card figure {
  margin: 0;
  display: block;
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.card figure img {
  margin: 0;
  display: block;
  position: absolute;
  width: 170px;
  height: 250px;
}

.back {
  transform: rotateY(180deg);
}

.flip {
  transform: rotateY(180deg);
}
</style>
