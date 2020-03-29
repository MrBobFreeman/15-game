<template>
  <section class="container">
    <div>
      <transition name="fade">
        <game v-if="isGameIsOn" @endGame="onEndGame" />
      </transition>
    </div>

    <transition name="fade">
      <div v-if="greeting" class="message">
        <h1 class="mb-20">Привет дорогой друг!</h1>
        <span class="mb-20"
          >Хочешь поиграть в интересную игру из моего детства? Привила просты -
          нужно собрать 15 цифр подряд, начиная с левого верхнего угла, в нем
          должна быть цифра 1 и далее слева на право. В конце поля, в правом
          нижнем углу, предпоследняя ячейка должна быть под цифрой 15, а
          последняя пустая.</span
        >
        <button class="btn mx-20" @click="onStartGame">Начать игру</button>
      </div>
    </transition>

    <transition name="fade">
      <div v-if="victory" class="message">
        <h1 class="mb-20">Поздравляю, Вы победили!</h1>
        <span class="mb-20">
          Ваш результат:
        </span>
        <game-info class="mb-20" :step="result.step" :time="result.time" />

        <button class="btn mx-20" @click="onStartGame">
          Начать новую игру
        </button>
      </div>
    </transition>
  </section>
</template>

<script>
import Game from "~/components/Game.vue";
import GameInfo from "~/components/GameInfo";

export default {
  components: {
    Game,
    GameInfo
  },
  data: () => ({
    result: {},
    greeting: true,
    victory: false
  }),

  computed: {
    isGameIsOn() {
      return !this.greeting && !this.victory;
    }
  },
  methods: {
    onStartGame() {
      this.greeting = false;
      this.victory = false;
    },
    onEndGame(result) {
      this.result = result;
      this.victory = true;
    }
  }
};
</script>

<style lang="scss">
@import "~assets/scss/main";

.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.message {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 600px;
  margin: 10px;
  line-height: 115%;
  font-size: $fontSizeMiddle;
  text-align: justify;
}

.fade-enter-active,
.fade-enter-to {
  transition: opacity 3s;
}
.fade-enter,
.fade-leave {
  opacity: 0;
}
</style>
