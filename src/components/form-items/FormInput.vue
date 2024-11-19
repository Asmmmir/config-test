<template>
  <div>
    <label :for="name">{{ label }}</label>
    <input v-bind="$attrs" v-model="inputValue" @input="removeSpaces" />
  </div>
</template>

<script>
export default {
  name: "FormInput",
  props: {
    modelValue: {
      type: [String],
      default: "",
    },
    label: String,
    name: String,
  },
  data() {
    return {
      inputValue: this.modelValue,
    };
  },
  methods: {
    removeSpaces() {
      this.inputValue = this.inputValue.replace(/\s+/g, "");
      this.$emit("update:modelValue", this.inputValue);
    },
  },
};
</script>
