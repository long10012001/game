<template>
  <main-app
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  ></main-app>
  <interactive-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinished="onGetResult"
  ></interactive-screen>

  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  ></result-screen>
</template>

<script>
import Main from "./components/MainScreen.vue";
import Interactive from "./components/InteractiveScreen.vue";
import Result from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: 0,
    };
  },
  components: {
    MainApp: Main,
    InteractiveScreen: Interactive,
    ResultScreen: Result,
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log(config);

      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCard = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCard = [...firstCard];
      const cards = [...firstCard, ...secondCard];
      this.settings.cardsContext = shuffled(cards);

      this.settings.startedAt = new Date().getTime();
      console.log(this.settings.cardsContext);
      // Data ready

      this.statusMatch = "match";
    },
    onGetResult() {
      // lấy thời gian
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.statusMatch = "result";
    },
  },
};
</script>
