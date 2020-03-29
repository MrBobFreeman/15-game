<template>
  <div class="game__info">
    <div class="game__info-item">
      <span class="mb-10">Ходов</span><span>{{ step }}</span>
    </div>
    <div class="game__info-item">
      <span class="mb-10">Время</span><span>{{ timer }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    step: {
      type: Number,
      required: true,
      default: 0
    },
    time: {
      type: Number,
      required: true,
      default: 0
    }
  },
  computed: {
    timer() {
      let minutes = Math.floor(this.time / 60);
      let seconds = Math.floor(this.time % 60);

      return `${String(minutes).padStart(2, "0")}:${String(seconds).padStart(
        2,
        "0"
      )}`;
    }
  }
};
</script>

<style lang="scss">
@import "~assets/scss/variables";

.game__info {
  display: flex;
  justify-content: space-around;
  padding: 20px 0;
  width: 350px;
  &-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 30%;
    padding: 15px;
    background-color: $bgColorSecondary;
    :last-child {
      font-size: $fontSizeMiddle;
    }
  }
}
</style>
