<template>
  <div
    class="card"
    :class="{ 'card-disabled': isDisabled }"
    :style="{
      height: `${(550 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((550 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((550 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div
      class="card_inner"
      :class="{ 'is-flipped': isFlip }"
      @click="onToggleFlipCard"
    >
      <div class="card_face card_face_front">
        <div
          class="card_content"
          :style="{
            backgroundSize: `${
              (((896 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card_face card_face_back">
        <div
          class="card_content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceURL)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imgBackFaceURL: {
      type: String,
      required: true,
    },
    card: { type: [String, Number, Array, Object] },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      isFlip: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false;
      this.isFlip = !this.isFlip;
      if (this.isFlip) {
        this.$emit("onFlip", this.card);
      }
    },

    onFlipBackCard() {
      this.isFlip = false;
    },
    onEnabledDisableMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-bottom: 2rem;
  margin-right: 1rem;
}

.card_inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card_inner.is-flipped {
  transform: rotateY(-180deg);
}

.card_face {
  position: absolute;
  width: 100%;
  height: 120%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;

  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card_face_front .card_content {
  background: url("../assets/images/download.png") no-repeat center center;
  background-color: var(--card);
  height: 100%;
  width: 100%;
}

.card_face_back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card_face_back .card_content {
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}

.card .card-disabled .card_inner {
  cursor: default;
}
</style>
