<template>
  <div ref="display" class="display">
    <div>Hello, my name is Shaural, welcome to my website...</div>
    <div
      v-for="(line, index) in displayText"
      :key="line + index"
      id="line"
      v-html="line"
    ></div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import store from "../store";

export default {
  name: "Display",
  computed: {
    ...mapState(["displayText", "path"]),
    displayTextLength: function() {
      return this.displayText.length;
    }
  },
  methods: {
    // TODO: Not scrolling all the way till the end, fix this
    scrollToEnd: function() {
      this.$refs.display.scrollTop = this.$refs.display.scrollHeight;
    },
    async defaultCommand(command) {
      console.log(command);
      await store.dispatch("commandHandler", command);
    }
  },
  watch: {
    displayTextLength: function() {
      this.scrollToEnd();
    }
  },
  created: function() {
    window.defaultCommand = this.defaultCommand;
  }
};
</script>

<style scoped>
.display {
  overflow-y: auto;
  word-wrap: break-word;
  max-height: 95%;
  -ms-overflow-style: none; /* Hide scrollbar for IE and Edge */
  scrollbar-width: none; /* Hide scrollbar for Firefox */
}

/* Hide scrollbar for Chrome, Safari and Opera */
.display::-webkit-scrollbar {
  display: none;
}

#line {
  display: flex;
  text-align: left;
  word-break: break-all;
}
</style>
