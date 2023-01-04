<template>
  <mainScreen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)" />
  <interactScreen v-if="statusMatch === 'match'" :cardsContext="settings.cardsContext" @onFinish="onGetResult" />
  <resultScreen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="onStart" />
  <copyrightScreen />
</template>

<script>
import mainScreen from "./components/mainScreen.vue";
import interactScreen from "./components/interactScreen.vue";
import resultScreen from "./components/resultScreen.vue";
import copyrightScreen from "./components/copyrightScreen.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0
    };
  },
  components: {
    mainScreen,
    interactScreen,
    resultScreen,
    copyrightScreen
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from({ length: this.settings.totalOfBlocks / 2 }, (_, i) => i + 1);
      const secondCards = Array.from({ length: this.settings.totalOfBlocks / 2 }, (_, i) => i + 1);
      const cards = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffled(shuffled(cards));
      this.settings.startedAt = new Date().getTime();
      this.statusMatch = "match";
    },

    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.statusMatch = "result";
    },

    onStart() {
      this.statusMatch = 'default';
      console.log("Start again");
    }
  },
};
</script>
