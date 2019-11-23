<template>
  <div class="col s3">
    <div class="card-container">
      <div
        id="cardElement"
        class="card"
        :class="{ flip : cardObj.isFaceUp }"
        @click="flipCard"
        @transitionstart="onTransitionStart"
        @transitionend="onTransitionEnd"
      >
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
import EventBus from "../EventBus";

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
      EventBus.$emit("flip-card", this.cardObj);
    },
    onTransitionStart() {
      EventBus.$emit("card-transition-start", this.cardObj);
    },
    onTransitionEnd() {
      EventBus.$emit("card-transition-end", this.cardObj);
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
  margin: 0.5em 0;
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
  width: 100%;
  height: 100%;
}

.back {
  transform: rotateY(180deg);
}

.flip {
  transform: rotateY(180deg);
}
</style>
