<template>
  <div>
    <label :for="name">{{ label }}</label>
    <input :value="modelValue" v-bind="$attrs" v-model.trim="inputValue" />
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
};
</script>
