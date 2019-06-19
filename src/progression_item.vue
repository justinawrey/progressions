<template>
  <div class="progression-item">
    <a
      class="progression-item-title"
      :class="{'progression-link-disabled': !link}"
      :href="link"
    >{{ title }}</a>
    <el-steps simple class="progression-item-steps" finish-status="success" :active="value">
      <el-step
        v-for="(step, index) in steps"
        :title="step"
        :key="index"
        @click.native="setValue(index)"
        class="progression-item-step"
        icon="none"
      />
    </el-steps>
    <progression-link :title="title" :type="type" @change-link="changeLink"/>
  </div>
</template>

<script>
import ProgressionLink from "./progression_link";

export default {
  components: {
    ProgressionLink
  },
  props: {
    title: {
      type: String,
      required: true
    },
    steps: {
      type: Array,
      required: false,
      default: () => []
    },
    default: {
      type: Number,
      required: false,
      default: 0
    },
    link: {
      type: String,
      required: true
    },
    type: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      value: this.default
    };
  },
  computed: {
    hasSteps() {
      return this.steps.length > 0;
    },
    hasLink() {
      console.log(!!link);
    }
  },
  methods: {
    setValue(index) {
      this.value = index;
    },
    changeLink(title, link, type) {
      this.$emit("change-link", title, link, type);
    }
  }
};
</script>

<style scoped>
.progression-item {
  display: flex;
  align-items: center;
  margin-right: 60px;
}

.progression-item-steps {
  flex-grow: 1;
  margin-top: 10x;
  margin-bottom: 10px;
  font-size: 5px;
}

.progression-item-step {
  cursor: pointer;
}

.progression-item-title {
  flex-shrink: 1;
  width: 150px;
  text-align: right;
  margin-right: 30px;
  text-decoration: none;
  color: black;
}

.progression-item-title:hover {
  text-decoration: underline;
  opacity: 0.5;
}

.progression-link-disabled {
  pointer-events: none;
  cursor: default;
}
</style>
