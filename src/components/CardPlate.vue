<template>
  <div>
    <div class="row container">
      <Card v-for="card in cards" :key="card.id" :cardObj="card" />
    </div>
  </div>
</template>

<script>
import Card from "./Card";

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
  }
};
</script>