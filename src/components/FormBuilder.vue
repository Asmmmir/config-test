<template>
  <div class="form-builder">
    <form class="form" @submit.prevent="handleSubmit">
      <div class="form-config">
        <div v-for="(formConfig, formKey) in config">
          <h2>{{ formConfig.name }}</h2>
          <div v-for="field in formConfig.items">
            <component
              :label="field.label"
              :options="field.additional?.options"
              :is="components[field.type]"
              :name="`${formKey}.${field.name}`"
              v-model="formData[formKey][field.name]"
            />
          </div>
        </div>
        {{ formData }}
      </div>

      <button type="submit">Отправить</button>
      <button @click="onReset" type="reset">Стереть</button>
    </form>
  </div>
</template>

<script>
import FormInput from "@/components/form-items/FormInput.vue";
import FormSelect from "@/components/form-items/FormSelect.vue";
import FormRadio from "@/components/form-items/FormRadio.vue";
import FormPassword from "@/components/form-items/FormPassword.vue";
import { reactive } from "vue";

const components = {
  password: FormPassword,
  radio: FormRadio,
  select: FormSelect,
  input: FormInput,
};

export default {
  name: "FormBuilder",
  props: {
    config: {
      type: Object,
      required: true,
    },
    onSubmit: {
      type: Function,
      required: true,
    },
  },
  components: {
    password: FormPassword,
    radio: FormRadio,
    select: FormSelect,
    input: FormInput,
  },

  setup(props) {
    const formData = reactive({
      parent: {},
      child: {},
    });

    // Валидация 

    function validate() {
      let isValid = true;

      for (let [formKey, formValue] of Object.entries(props.config)) {
        formValue.items.forEach((field) => {
          const fieldValue = formData[formKey][field.name];

          if (!fieldValue) {
            isValid = false;
            return;
          }

          const parentField = field.additional?.parent;

          if (parentField) {
            const parentValue = formData[formKey][parentField];

            if (fieldValue != parentValue) {
              isValid = false;
              return;
            }
          }
        });
      }

      return isValid;
    }

    // Сброс 
    
    function onReset() {
      for (const formKey in formData) {
        for (const formValue in formData[formKey]) {
          formData[formKey][formValue] = undefined;
        }
      }
    }

    // Подготовка к отправке 

    function toResult(obj) {
      const result = {};

      for (const [formKey, formValue] of Object.entries(props.config)) {
        result[formKey] = {};

        formValue.items.forEach((field) => {
          if (field.type === "password" && field.additional?.parent) {
            return;
          }

          result[formKey][field.name] = obj[formKey][field.name];
        });
      }

      return result;
    }

    //Отправка формы

    function handleSubmit() {
      if (!validate()) {
        alert("Неверно введены поля!");
        return;
      }

      const preparedData = toResult(formData);
      props.onSubmit(preparedData);
    }

    return {
      formData,
      config: props.config,
      components,
      onReset,
      handleSubmit,
    };
  },
};
</script>

<style scoped></style>
