<script lang="ts">
import { defineComponent } from "vue";
import { useMainStore } from "@/stores/zss";
import { Panels, useUIStore } from "@/stores/ui";
import {
  minOctave,
  maxOctave,
  incrementOctave,
  decrementOctave,
} from "../library/res/Keymap";
import PanelHeader from "@/components/PanelHeader.vue";
import TransportBar from "./TransportBar.vue";
import Pager from "./Pager.vue";

export default defineComponent({
  emits: ["nextPattern", "prevPattern"],
  components: { PanelHeader, TransportBar, Pager },
  setup() {
    const main = useMainStore();
    const ui = useUIStore();
    return {
      main,
      ui,
      minOctave,
      maxOctave,
      decrementOctave,
      incrementOctave,
      Panels,
    };
  },
  data() {
    return { windowWidth: window.innerWidth };
  },
  mounted() {
    window.addEventListener("resize", this.onResize);
  },
  destroyed() {
    window.removeEventListener("resize", this.onResize);
  },
  methods: {
    onResize(e: Event) {
      this.windowWidth = (e.target as Window).innerWidth;
    },
    formatIndex(index: number): string {
      return index < 10 ? "0" + index.toString() : index.toString();
    },
    goBack() {
      this.$router.go(-1);
    },
  },
  computed: {
    patterns() {
      return JSON.stringify(this.main.song?.getPatternIDs());
    },
  },
});
</script>

<template>
  <span class="mobile-hide-small">
    <PanelHeader title="Song - Octave" :id="Panels.song">
      <template #option> {{ ui.currentOctave }} </template>
      <template #control>
        <Pager
          title="octave"
          :value="ui.currentOctave"
          custom-hint-back="Decrease octave (Num -)"
          custom-hint-forward="Increase octave (Num +)"
          :min="minOctave"
          :max="maxOctave"
          @previous="decrementOctave()"
          @next="incrementOctave()"
        ></Pager>
      </template>
    </PanelHeader>
  </span>

  <div class="panel-content mobile-hide-small">
    <div class="container song-info-row">
      <span>[{{ main.song.name.toLowerCase() }}] &nbsp;&nbsp;</span>
      <code class="song-info">
        &nbsp;
        <span class="song-info-detail"
          >#sequences: {{ main.song?.sequences.length }}&nbsp;</span
        >
        <span class="song-info-detail">
          <span
            data-bs-toggle="tooltip"
            data-bs-placement="top"
            :title="patterns"
            >&nbsp;#patterns: {{ main.song?.patterns.length }} &nbsp;
          </span>
        </span>
      </code>
    </div>
    <div class="quickhelp">
      <div>Press any alphanumeric key to play a note.</div>
      <div>Press Esc to toggle edit mode / window focus.</div>
    </div>
    <span class="me-4"></span>
  </div>
  <template v-if="windowWidth >= 769">
    <TransportBar></TransportBar>
  </template>
</template>

<style scoped>
.song-info-row {
  margin-left: 0px;
  padding-left: 2px;
}
.song-info {
  font-size: 0.92em;
  color: burlywood;
  margin-top: 40px;
}
.quickhelp {
  font-size: 1em;
  color: grey;
  margin: 10px 0 0 3px;
}

@media (min-width: 992px) {
  .panel-content {
    margin-left: 15px;
  }
}

@media (max-width: 1350px) {
  .song-info {
    display: none;
  }
}
</style>
