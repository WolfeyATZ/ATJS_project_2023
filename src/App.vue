<template>
  <h1>Memory Game</h1>
  <section class="game-board">
    <Card v-for="(card, index) in cardList"
          :key="`card-${index}`"
          :matched="card.matched"
          :value="card.value"
          :visible="card.visible"
          :position="card.position"
          @select-card="flipCard">

    </Card>
  </section>
  <h2>{{ status }}</h2>
  <button @click="restartGame">Shuffle Cards</button>
</template>

<script>
import _ from 'lodash'
import { computed, ref, watch} from 'vue'
import Card from "./components/Card.vue";

export default {
  name: `App`,
  components: {
    Card
  },
  setup() {
    const cardList = ref([])
    const userSelection = ref([])
    const status = computed(() => {
      if (remainingPairs.value === 0) {
        return 'Player wins!'
      } else {
        return `Remaining Pairs: ${remainingPairs.value}`
      }
    })

    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter (card => card.matched === false).length

      return remainingCards / 2
    })

    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value)
    }

    const restartGame = () => {
      shuffleCards()

      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          matched: false,
          position: index,
          visible: false
        }
      })
    }

    const cardItems = ['Moon', 'Earth', 'Jupiter', 'Mars', 'Neptun', 'Venus', 'Pluto', 'Sun']

    cardItems.forEach(item => {
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false
      })

      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false
      })
    })

    cardList.value = cardList.value.map((card, index) => {
      return {
        ...card,
        position: index
      }
    })

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true

      if (userSelection.value[0]) {
        if ( userSelection.value[0].position === payload.position && userSelection.value[0].faceValue === payload.faceValue ) {
         return
        } else {
          userSelection.value[1] = payload

        }

      } else {
        userSelection.value[0] = payload
      }
    }

    watch(userSelection, currentValue => {
      if (currentValue.length === 2) {
        const cardOne = currentValue[0]
        const cardTwo = currentValue[1]

        if (cardOne.faceValue === cardTwo.faceValue) {

          cardList.value[cardOne.position].matched = true
          cardList.value[cardTwo.position].matched = true
        } else {
          setTimeout(() => {
          cardList.value[cardOne.position].visible = false
          cardList.value[cardTwo.position].visible = false

          }, 1000)
        }


        userSelection.value.length = 0
      }
    }, { deep: true })

    return {
      cardList,
      flipCard,
      userSelection,
      status,
      shuffleCards,
      restartGame
    }
  }
}
</script>

<style>

body {
  background-image: url('../public/images/Universe.png');
}

#app {
  background-color: #020917;
  padding-left: 100px;
  padding-right: 100px;
  height: 88vh;
  border-radius: 50px;
  background-image: url('../public/images/Galaxy.gif');
  background-size: 1000px;
  background-position: center;
  color: white;

}

h1 {
  margin-top: 0;
  font-family: "arial";
  font-weight: bold;
  background-color: #349dca;
  border-radius: 50px;
  padding-top: 10px;
  padding-bottom: 10px;
}

h2 {
  text-decoration: underline;
  color: #fdbd24;
}

.game-board {
  display: grid;
  grid-template-columns: 120px 120px 120px 120px;
  grid-template-rows: 120px 120px 120px 120px;
  grid-column-gap: 24px;
  grid-row-gap: 24px;
  justify-content: center;
}

button {
  color: mediumpurple;
}

</style>
