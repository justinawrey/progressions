<template>
  <div>
    <i v-popover:popover class="el-icon-paperclip progression-link" @click="toggle"/>
    <el-popover
      ref="popover"
      placement="bottom-start"
      width="300"
      trigger="manual"
      v-model="visible"
    >
      <div>
        <div>Change Link</div>
        <el-input
          ref="link"
          size="medium"
          clearable
          v-model="link"
          @keyup.enter.native="emitChangeLinkEvent"
        />
      </div>
    </el-popover>
  </div>
</template>

<script>
export default {
  props: {
    title: {
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
      link: "",
      visible: false
    };
  },
  methods: {
    toggle() {
      this.visible = !this.visible;
      this.$nextTick().then(() => this.$refs.link.focus());
    },
    emitChangeLinkEvent() {
      this.visible = !this.visible;
      this.$emit("change-link", this.title, this.link, this.type);
    }
  }
};
</script>

<style>
.progression-link {
  margin-left: 10px;
}

.progression-link:hover {
  cursor: pointer;
  opacity: 0.5;
}
</style>
