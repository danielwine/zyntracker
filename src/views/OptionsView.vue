<script lang="ts">
import { useUIStore, Panels } from "@/stores/ui";
import { defineComponent } from "vue";
import PanelHeader from "@/components/PanelHeader.vue";

export default defineComponent({
  setup() {
    const ui = useUIStore();
    return {
      ui,
      Panels,
    };
  },
  mounted() {
    this.ui.activePanel = Panels.options;
  },
  components: { PanelHeader },
});
</script>

<template>
  <PanelHeader title="Options" :id="Panels.options"></PanelHeader>
  <div class="p-4">
    <h4>General options</h4>
    <p></p>
    <!-- <h4>Specific options</h4> -->
    <!-- <p></p> -->
  </div>
</template>
