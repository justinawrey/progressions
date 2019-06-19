<template>
  <div class="progression-sidebar">
    <div
      class="progression-sidebar-item"
      v-for="(progression, index) in progressions"
      :key="index"
      @click="emitFocusEvent(progression)"
    >{{ progression }}</div>
  </div>
</template>

<script>
export default {
  props: {
    progressions: {
      type: Array,
      required: true
    }
  },
  methods: {
    emitFocusEvent(progression) {
      this.$emit("focus-progression", progression);
    }
  }
};
</script>

<style scoped>
.progression-sidebar {
  position: fixed;
  left: 2%;
  width: 15%;
}

.progression-sidebar-item {
  font-size: 13px;
  opacity: 0.5;
  cursor: pointer;
  transition: all 0.2s linear;
  border-left: solid black;
  padding-left: 9px;
  padding-top: 5px;
  padding-bottom: 5px;
}

.progression-sidebar-item:hover {
  opacity: 1;
}
</style>
