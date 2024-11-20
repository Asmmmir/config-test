<template>
  <div>
    <label :for="name">{{ label }}</label>
    <input :id="id" :name="name" :value="modelValue" v-bind="$attrs" v-model.trim="inputValue" />
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
    id: String
  },
  data() {
    return {
      inputValue: this.modelValue,
    };
  },
};
</script>
