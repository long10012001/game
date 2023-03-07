<template>
  <div class="screen">
    <div
      class="card_inner"
      :style="{
        width: `${
          ((((550 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-filp
        ref="card"
        v-for="(card, index) in cardsContext"
        :key="index"
        :card="{ index: index, value: card }"
        :imgBackFaceURL="`images/${card}.png`"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
      ></card-filp>
    </div>
  </div>
</template>

<script>
import CardFilp from "./Card.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFilp: CardFilp,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) {
        return false;
      }
      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[1].value === this.rules[0].value
      ) {
        console.log("đúng");
        // add class 'disabled' to card
        this.$refs.card[`${this.rules[0].index}`].onEnabledDisableMode();
        this.$refs.card[`${this.rules[1].index}`].onEnabledDisableMode();
        // reset rule[]
        this.rules = [];

        // check complete
        const disabledElement = document.querySelectorAll(
          ".screen .card-disabled"
        );
        if (
          disabledElement &&
          disabledElement.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinished");
          }, 1000);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[1].value !== this.rules[0].value
      ) {
        console.log("sai");
        // Đóng 2 thẻ sai
        setTimeout(() => {
          this.$refs.card[`${this.rules[0].index}`].onFlipBackCard();
          this.$refs.card[`${this.rules[1].index}`].onFlipBackCard();
          this.rules = [];
        }, 800);
        // reset rules = []
      } else return false;
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  /* width: 100%; */
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
  padding-left: 4%;
}

.card_inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
