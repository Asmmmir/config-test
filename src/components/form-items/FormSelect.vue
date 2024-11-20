<template>
  <div class="select">
    <p>{{ label }}</p>
    <select :value="modelValue" @change="$emit('update:modelValue', $event.target.value)">
      <option
        v-for="option in options"
        :key="option.value"
        :selected="option.selected"
        :value="option.value"
      >{{ option.text}}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: "FormSelect",

  props: {
    label: {
      type: String,
      default: ''
    },

    options: {
      type: Object,
      required: true
    },
    modelValue: {
      type: String,
    }
  },
 
  setup(){

    
  }
}
</script>

<style scoped>
.select {
  display: flex;
}

</style>
