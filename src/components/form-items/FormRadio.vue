<template>
  <fieldset>
    <legend>{{ label }}</legend>

    <div v-for="option in options" :key="option.value">
      <input
        required
        type="radio"
        :name="name"
        :value="option.value"
        :checked="option.selected"
        @change="$emit('update:modelValue', Number($event.target.value))" 
      />
      <label :for="option.value">{{ option.text }}</label>
    </div>
  </fieldset>
</template>

<script>
export default {
  name: "FormRadio",

  props: {
    label: {
      type: String,
      default: "",
    },

    options: {
      type: Object,
      required: true,
    },

    name:{
      type: String
    },
    modelValue: {
      type: [Number]
    }
  },
};
</script>

<style scoped></style>
