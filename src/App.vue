<template>
  <div id="app">
    <h1 class="title">
      <img class="vue-logo" src="https://vuejs.org/images/logo.png">
      Card Shuffling
    </h1>
    <div class="count-section" v-if="shuffleCount>0"># of Shuffles: {{ shuffleCount }}</div>
    <div class="speed-buttons">
      <button
        v-for="type in shuffleTypes"
        :key="type"
        class="button is-small"
        :class="{ 'is-light': shuffleSpeed != `shuffle${type}` }"
        @click="shuffleSpeed = `shuffle${type}`"
      >{{ type }}</button>
    </div>
    <div class="main-buttons">
      <button
        v-if="isDeckShuffled"
        @click="displayInitialDeck"
        class="button is-primary is-outlined"
      >
        Reset
        <i class="fas fa-undo"></i>
      </button>
      <button @click="shuffleDeck" class="button is-primary">
        Shuffle
        <i class="fas fa-random"></i>
      </button>
    </div>
    <div class="deck">
      <transition-group :name="shuffleSpeed" tag="div" class="deck">
        <div v-for="card in cards" :key="card.id" class="card" :class="suitColor[card.suit]">
          <span class="card__suit card__suit--top">{{ card.suit }}</span>
          <span class="card__number">{{ card.rank }}</span>
          <span class="card__suit card__suit--bottom">{{ card.suit }}</span>
        </div>
      </transition-group>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      ranks: ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"],
      suits: ["♥", "♦", "♠", "♣"],
      cards: [],
      suitColor: {
        "♠": "black",
        "♣": "black",
        "♦": "red",
        "♥": "red"
      },
      shuffleSpeed: "shuffleMedium",
      shuffleTypes: ["Slow", "Medium", "Fast"],
      isDeckShuffled: false,
      shuffleCount: 0
    };
  },
  created() {
    this.displayInitialDeck();
  },
  methods: {
    shuffleDeck() {
      for (let i = 0; i < this.cards.length; i++) {
        let randomIndex = Math.floor(Math.random() * i);
        let temp = this.cards[i];

        this.$set(this.cards, i, this.cards[randomIndex]);
        this.$set(this.cards, randomIndex, temp);
        this.isDeckShuffled = true;
      }
      this.shuffleCount++;
    },
    displayInitialDeck() {
      let id = 1;
      this.cards = [];
      for (let s = 0; s < this.suits.length; s++) {
        for (let r = 0; r < this.ranks.length; r++) {
          let card = {
            id: id,
            rank: this.ranks[r],
            suit: this.suits[s]
          };
          this.cards.push(card);
          id++;
        }
      }
      this.isDeckShuffled = false;
      this.shuffleCount = 0;
    }
  }
};
</script>

