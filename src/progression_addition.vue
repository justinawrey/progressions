<template>
  <div class="progression-addition">
    <el-popover
      ref="popover"
      placement="bottom-start"
      width="500"
      trigger="manual"
      v-model="visible"
    >
      <div class="progression-addition-header">New Task</div>
      <div class="progression-addition-field">
        <div class="progression-addition-field-label">Title</div>
        <el-input
          size="medium"
          clearable
          v-model="name"
          ref="name"
          @keyup.enter.native="emitAddProgressionEvent"
        />
      </div>
      <div class="progression-addition-field">
        <div class="progression-addition-field-label">Phabricator</div>
        <el-input
          size="medium"
          clearable
          v-model="phab"
          @keyup.enter.native="emitAddProgressionEvent"
        />
      </div>
      <div class="progression-addition-field">
        <div class="progression-addition-field-label">Jira</div>
        <el-input
          size="medium"
          clearable
          v-model="jira"
          @keyup.enter.native="emitAddProgressionEvent"
        />
      </div>
      <div class="progression-addition-field">
        <div class="progression-addition-field-label">Tech Spec</div>
        <el-input
          size="medium"
          clearable
          v-model="ts"
          @keyup.enter.native="emitAddProgressionEvent"
        />
      </div>
      <div class="progression-addition-submit-button">
        <el-button size="small" @click="emitAddProgressionEvent">Submit</el-button>
      </div>
    </el-popover>
    <i
      v-popover:popover
      class="el-icon-circle-plus-outline progression-addition-icon"
      @click="toggle"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      jira: "",
      ts: "",
      phab: "",
      visible: false
    };
  },
  methods: {
    emitAddProgressionEvent() {
      if (this.name) {
        this.visible = false;
        this.$emit("add-progression", {
          name: this.name,
          jira: this.jira,
          ts: this.ts,
          phab: this.phab
        });
      } else {
        this.$message({
          dangerouslyUseHTMLString: true,
          showClose: true,
          type: "error",
          duration: 2300,
          message: "<strong>Error: </strong> Empty title field"
        });
      }
    },
    toggle() {
      this.name = "";
      this.jira = "";
      this.ts = "";
      this.phab = "";
      this.visible = !this.visible;
      this.$nextTick().then(() => this.$refs.name.focus());
    }
  }
};
</script>

<style scoped>
.progression-addition {
  position: fixed;
  top: 2%;
  right: 2%;
}

.progression-addition-icon {
  font-size: 40px;
}

.progression-addition-icon:hover {
  opacity: 0.5;
  cursor: pointer;
}

.progression-addition-field {
  display: flex;
  margin-top: 4px;
  margin-bottom: 4px;
  align-items: center;
  padding-right: 20px;
}

.progression-addition-field-label {
  width: 90px;
  text-align: right;
  margin: 10px;
}

.progression-addition-header {
  font-size: 20px;
  text-align: center;
  margin-bottom: 10px;
}

.progression-addition-submit-button {
  display: flex;
  justify-content: center;
  margin-top: 10px;
}
</style>
