<template>
  <div class="game">
    <button class="btn" @click="onNewGame">Новая игра</button>

    <GameInfo :step="stepCounter" :time="timer" />

    <transition-group name="animation" tag="div" class="game__field">
      <div
        v-for="number in field"
        :key="number"
        :class="{ game__cell: true, empty: !number }"
        @click="onMoveCell(number)"
      >
        <div v-if="number">{{ number }}</div>
      </div>
    </transition-group>
  </div>
</template>

<script>
import GameInfo from "~/components/GameInfo";

export default {
  components: {
    GameInfo
  },

  data: () => ({
    timer: 0,
    timeInterval: null,
    stepCounter: 0,
    field: []
  }),

  created() {
    this.mixField();
  },

  beforeDestroy() {
    this.stopTimer();
  },

  computed: {
    indexEmptyCell() {
      return this.field.indexOf(0);
    },

    victoryField() {
      const field = this.createField().slice(1);
      field.push(0);

      return field;
    }
  },

  methods: {
    isMobile(number) {
      const difference = Math.abs(
        this.getIndexCell(number) - this.indexEmptyCell
      );
      return difference === 1 || difference == 4;
    },

    onMoveCell(number) {
      if (!this.isMobile(number)) {
        return;
      }

      const indexCell = this.getIndexCell(number);

      this.startTimer();
      this.$set(this.field, this.indexEmptyCell, number);
      this.$set(this.field, indexCell, 0);
      this.stepCounter += 1;
      this.checkVictory();
    },

    onNewGame() {
      this.stopTimer();
      this.timer = 0;
      this.stepCounter = 0;
      this.mixField();
    },

    getIndexCell(number) {
      return this.field.indexOf(number);
    },

    createField() {
      const field = [];
      for (let i = 0; i <= 15; i += 1) {
        field.push(i);
      }
      return field;
    },

    mixField() {
      const field = this.createField();
      this.field = field.sort(() => Math.random() - 0.5);
    },

    startTimer() {
      if (!this.timeInterval) {
        this.timeInterval = setInterval(() => {
          this.timer += 1;
        }, 1000);
      }
    },

    stopTimer() {
      if (this.timeInterval !== null) {
        clearInterval(this.timeInterval);
        this.timeInterval = null;
      }
    },

    checkVictory() {
      const fieldString = this.field.join();
      const victoryFieldString = this.victoryField.join();

      if (fieldString === victoryFieldString) {
        this.victoryGame();
      }
    },

    victoryGame() {
      const result = { step: this.stepCounter, time: this.timer };
      this.$emit("endGame", result);
    }
  }
};
</script>

<style lang="scss">
@import "~assets/scss/variables";

.game {
  padding: 20px;
  background-color: $bgColorPrimary;
  &__field {
    display: flex;
    flex-wrap: wrap;
    width: 350px;
  }
  &__cell {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-basis: 80px;
    height: 80px;
    margin: 2px;
    font-size: $fontSizeLarge;
    background-color: $bgColorSecondary;
    &:hover {
      background-color: $colorHoverItem;
      cursor: pointer;
    }
  }
}

.empty {
  opacity: 0;
  &:hover {
    cursor: unset !important;
  }
}

.animation-move {
  transition: all 0.15s ease-in-out;
}
</style>
