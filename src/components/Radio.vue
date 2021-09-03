<template>
  <div class="radio-container">
    <label class="label">
      <input class="input" type="radio" :value="label" v-model="v" />
      <span class="text"><slot></slot></span>
    </label>
  </div>
</template>

<script>
export default {
  props: {
    label: [String, Number, Boolean],
    value: [String, Number, Boolean]
  },
  computed: {
    v: {
      get() {
        return this.value
      },
      set(newVal) {
        this.$emit('input', newVal)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .radio-container {
    .label {
      cursor: pointer;
    }
    .text {
      padding: 0 0 0 8px;
    }
  }
</style>
