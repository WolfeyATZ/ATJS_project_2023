<script>
  export default {
    props: {
      matched: {
        type: Boolean,
        default: false
      },
      position: {
        type: Number,
        required: true
      },
      value: {
        type: String,
        required: true
      },
      visible: {
        type: Boolean,
        default: false
      }
    },
    setup(props, context) {
      const selectCard = () => {
        context.emit(`select-card`, {
          position: props.position,
          faceValue: props.value

        })
      }

      return {
        selectCard
      }
    }
  }

</script>


<template>

  <div class="card" @click="selectCard">
    <div v-if="visible" class="card-face is-front">
      <img :src="`/images/${value}.png`" :alt="value">

      <img v-if="matched" src="/images/checkmark.png" class="checkmark"/>

    </div>

    <div v-else class="card-face is-back">

    </div>
  </div>

</template>


<style>

.card {
  border: 3px solid #ccc;
  border-radius: 20px;
  position: relative;
  padding: 0;
}

.checkmark {
  height: 80px;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 20px;
  size: 50px;

}

.card-face.is-front {
  background-color: #411d8e;
  color: white;
  width: 115px;
}

.card-face.is-front img{
  border-radius: 20px;
  width: 80px;
  height: 107px
}


.card-face.is-back {
  background-image: url('../public/images/Star.png');
  background-size: 110px;
  background-color: rgba(0, 12, 23, 0.78);
  background-position-x: center;
  background-position-y: center;
  border-radius: 20px;
}

.checkmark {
  position: absolute;
  right: 5px;
  bottom: 5px;
  size: 20px;
}

</style>