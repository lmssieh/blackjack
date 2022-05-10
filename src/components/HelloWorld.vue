<script setup lang="ts">
import { ref, reactive, computed } from 'vue'
import CardVue from './Card.vue'

defineProps<{ msg: string }>()

type Card = {
  value: number | string,
  type: string
}

type Player = {
  cards: Card[],
  cardsTotal: number
}
const card = (value: number | string, type: string) => ({ value, type })

function generateDeck(types: string[], values: (string | number)[]) {
  const newArr: Card[] = [];
  types.forEach(type => {
    values.forEach(value => {
      newArr.push(card(value, type))
    })
  });
  return newArr
};
function shuffle(arr: Card[]): Card[] {
  const newArr = [...arr]
  for (let i = 0; i < newArr.length; i++) {
    const random = Math.floor(Math.random() * newArr.length);
    const temp = newArr[i];
    newArr[i] = newArr[random]
    newArr[random] = temp
  }
  return newArr
}

const drawCard = () => {
  if (!shuffledDeck.value.length) shuffledDeck.value = shuffle(deck)
  return shuffledDeck.value.shift()
}
const dealCard = (player: Player) => {
  player.cards.push(drawCard())
}

const user: Player = reactive({
  cards: [],
  cardsTotal: 0
});

const dealer: Player = reactive({
  cards: [],
  cardsTotal: 0
});

const types = ['diamonds', 'hearts', 'clubs', 'spades'];
const values = ['A', 2, 3, 4, 5, 6, 7, 8, 9, 'J', 'Q', 'K'];

const deck = generateDeck(types, values)
const shuffledDeck = ref(shuffle(deck))

const calculateCardTrueValue = (value: number | string) => {
  if (value == 'A') {
    if (user.cardsTotal < 10) return 10
    return 1
  } else if (value == 'J' || value == 'Q' || value == 'K') {
    return 10
  } else {
    return Number(value)
  }
}

const userCardsTotal = computed(() => {
  const total: number = user.cards
    .map((card: Card) => card.value)
    .reduce(((prev, next) => calculateCardTrueValue(prev) + calculateCardTrueValue(next)));
  user.cardsTotal = total;
  return total
})

const dealerCardsTotal = computed(() => {
  const total: number = dealer.cards
    .map((card: Card) => card.value)
    .reduce(((prev, next) => calculateCardTrueValue(prev) + calculateCardTrueValue(next)));
  dealer.cardsTotal = total;
  return total
})

function init() {
  dealCard(dealer)
  dealCard(user)
  dealCard(dealer)
  dealCard(user)
  dealCard(dealer)
}

function reset() {
  user.cards = []
  dealer.cards = []
  init()
}

init()
console.log();
</script>

<template>
    <div class="flex justify-center flex-wrap">
      <CardVue v-for="card in user.cards" :card="card" :key="card" class="mb-4 mr-4" />
      <div>
              <div class="bg-red-500 text-white p-2 text-xl height-auto font-bold block rounded-full ">
        {{ userCardsTotal }}
      </div>
      </div>
    </div>
    <div class="flex justify-center flex-wrap">
      <CardVue v-for="card in dealer.cards" :card="card" :key="card" class="mb-4 mr-4" />
      <div>
              <div class="bg-red-500 text-white text-xl height-auto font-bold block rounded-full ">
        {{ dealerCardsTotal }}
      </div>
      </div>
    </div>
  <button type="button" @click="reset()">Restart</button>

</template>

<style scoped>
a {
  color: #42b983;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
}
</style>
