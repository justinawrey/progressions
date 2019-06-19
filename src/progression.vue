<template>
  <el-card class="progression">
    <template #header>
      <div class="progression-header">
        <div class="progression-header-title">{{ title }}</div>
        <i class="el-icon-delete progression-header-trash" @click="emitRemoveProgression"/>
      </div>
    </template>
    <progression-item
      title="Phabricator"
      type="phab"
      :link="links['phab']"
      :steps="!!links['phab'] ? ['Not Started', 'Diff Out', 'In Revision', 'Ready to Land'] : []"
      @change-link="changeLink"
    />
    <progression-item
      title="Jira"
      type="jira"
      :link="links['jira']"
      :steps="!!links['jira'] ? ['Not Started', 'In Progress', 'Code Complete', 'Ready For Testing'] : []"
      @change-link="changeLink"
    />
    <progression-item
      title="Tech Spec"
      type="ts"
      :link="links['ts']"
      :steps="!!links['ts'] ? ['Not Started', 'Spec Out', 'In Revision', 'In Team Drive'] : []"
      @change-link="changeLink"
    />
  </el-card>
</template>

<script>
import ProgressionItem from "./progression_item";

export default {
  props: {
    title: {
      type: String,
      required: true
    },
    links: {
      type: Object,
      required: true
    }
  },
  components: {
    ProgressionItem
  },
  methods: {
    emitRemoveProgression() {
      this.$emit("remove-progression");
    },
    changeLink(_, link, type) {
      this.$emit("change-link", this.title, link, type);
    }
  }
};
</script>

<style scoped>
.progression {
  margin-top: 20px;
  margin-bottom: 20px;
}

.progression-header {
  position: relative;
}

.progression-header-title {
  font-size: 20px;
  text-align: center;
}

.progression-header-trash {
  position: absolute;
  top: 3px;
  right: 0;
  font-size: 20px;
}

.progression-header-trash:hover {
  opacity: 0.5;
  cursor: pointer;
}
</style>
