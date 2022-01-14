<template>
  <div>
    <div class="List">
      <div v-for="(item, index) in model.items" :key="index" class="uk-flex">
        <input
          v-model="model.items[index]"
          placeholder="Value"
          class="input-group__field"
          ref="input"
        />

        <div>
          <i
            class="input-group__item schema__trash uk-icon-chevron-up"
            :style="{ opacity: index > 0 ? 1 : 0 }"
            @click="moveUp(index)"
          ></i>
          <i
            class="input-group__item schema__trash uk-icon-chevron-down"
            :style="{ opacity: index < model.items.length - 1 ? 1 : 0 }"
            @click="moveDown(index)"
          ></i>
          <i
            class="input-group__item schema__trash uk-icon-trash"
            @click="removeItem(index)"
        ></i>
        </div>
      </div>
    </div>
    <a
      class="uk-button uk-button-primary uk-button-small uk-margin-top"
      @click="addItem"
    >
      <i class="uk-icon-plus"></i> Add
    </a>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  methods: {
    initWith() {
      return {
        plugin: "string-array",
        items: [],
      };
    },
    pluginCreated() {
      // eslint-disable-next-line
    },
    addItem() {
      this.model.items.push("");
      setTimeout(() => {
        const input = this.$refs.input[this.$refs.input.length - 1];
        input && input.focus();
      }, 50);
    },
    removeItem(index) {
        this.model.items.splice(index, 1);
    },
    moveDown(index) {
      if (index >= this.model.items.length - 1) {
        return;
      }
      this.model.items.splice(
        index,
        2,
        this.model.items[index + 1],
        this.model.items[index]
      );
    },
    moveUp(index) {
      if (index <= 0) {
        return;
      }
      this.model.items.splice(
        index - 1,
        2,
        this.model.items[index],
        this.model.items[index - 1]
      );
    },
  },
  watch: {
    model: {
      handler: function (value) {
        this.$emit("changed-model", value);
      },
      deep: true,
    },
  },
};
</script>

<style>
.List {
  row-gap: 8px;
  display: flex;
  flex-direction: column;
}
</style>
