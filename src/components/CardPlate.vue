<template>
  <div>
    <div class="row" v-for="i in 2" :key="i">
      <div class="col s12 offset-s2">
        <div v-for="j in 4" :key="j">
          <Card
            :cardObj="cards[i > 1 ? j + 3 : j - 1]"
            :key="cards[i > 1 ? j + 3 : j - 1].id"
            @flip-card="flipCard"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Card from "./Card.vue";
import { log } from 'util';

export default {
  name: "CardPlate",
  props: {
    cards: {
      type: Array,
      required: true
    }
  },
  components: {
    Card
  },
  methods: {
    flipCard(id) {
      this.$emit("flip-card", id);
    }
  }
};
</script>