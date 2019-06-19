<template>
  <div>
    <progression-addition @add-progression="addProgression"/>
    <progression-sidebar :progressions="progressions" @focus-progression="focusProgression"/>
    <div class="container">
      <div v-if="hasProgressions">
        <progression
          v-for="(progression, index) in progressions"
          :key="index"
          :title="progression"
          :ref="progression"
          :links="links[progression]"
          @remove-progression="removeProgression(index)"
          @change-link="changeLink"
        />
      </div>
      <div v-else class="empty">
        <i class="el-icon-tableware empty-icon"/>
        <div class="italic">Time to relax...</div>
      </div>
    </div>
  </div>
</template>

<script>
import Progression from "./progression";
import ProgressionSidebar from "./progression_sidebar";
import ProgressionAddition from "./progression_addition";

export default {
  components: {
    Progression,
    ProgressionSidebar,
    ProgressionAddition
  },
  data() {
    return {
      progressions: [],
      links: {}
    };
  },
  computed: {
    hasProgressions() {
      return this.progressions.length > 0;
    }
  },
  methods: {
    addProgression(e) {
      const { name, jira, ts, phab } = e;
      this.$set(this.links, name, {});
      this.$set(this.links[name], "ts", ts || "");
      this.$set(this.links[name], "phab", phab || "");
      this.$set(this.links[name], "jira", jira || "");

      this.progressions.push(name);
      this.$nextTick().then(() => this.focusProgression(name));

      this.$message({
        dangerouslyUseHTMLString: true,
        showClose: true,
        type: "success",
        duration: 2300,
        message: `<strong>Added Task: </strong>${name}`
      });
    },
    removeProgression(index) {
      const task = this.progressions[index];
      this.progressions.splice(index, 1);
      this.$message({
        dangerouslyUseHTMLString: true,
        showClose: true,
        duration: 2300,
        message: `<strong>Deleted task: </strong>${task}`
      });
    },
    focusProgression(e) {
      const el = this.$refs[e][0].$el;
      el.scrollIntoView();
    },
    changeLink(title, link, type) {
      this.links[title][type] = link;
      this.$message({
        dangerouslyUseHTMLString: true,
        showClose: true,
        duration: 2300,
        type: "success",
        message: `Changed link for <strong>${type}</strong> to <strong>${link}</strong>`
      });
    }
  }
};
</script>

<style>
html {
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
    "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
}

html,
body {
  height: 100%;
}

body {
  margin: 0;
}

.container {
  margin-left: 15%;
  margin-right: 15%;
}

.empty {
  position: absolute;
  top: 50%;
  right: 50%;
  height: 300px;
  width: 300px;
  margin-top: -150px;
  margin-right: -150px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.empty-icon {
  font-size: 200px;
}

.italic {
  font-style: italic;
}
</style>
