<template>
  <div class="form-builder">
    <form class="form" @submit.prevent="onSubmit">
      <div class="form-config">
        <div v-for="(formConfig, formIndex) in config">
          <h2>{{ formConfig.name }}</h2>
          <div v-for="field in formConfig.items">
            <component
              :label="field.label"
              :options="field.additional?.options"
              :is="components[field.type]"
              :name="formIndex + field.name"
              v-model="formData[formIndex][field.name]"
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

function toResult(obj) {
  return {
    name: obj.name,
    gender: obj.gender,
    age: obj.age,
    pass: obj.pass,
  };
}

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
  },
  components: {
    password: FormPassword,
    radio: FormRadio,
    select: FormSelect,
    input: FormInput,
  },

  setup(props) {
    const formData = reactive({
      parent: {
        name: undefined,
        gender: undefined,
        age: undefined,
        pass: undefined,
      },
      child: {
        name: undefined,
        gender: undefined,
        age: undefined,
        pass: undefined,
      },
    });

    function validate(config) {
      let isValid = true;

      for (let [formKey, formValue] of Object.entries(config)) {
        formValue.items.forEach((field) => {
          const fieldValue = formData[formKey][field.name]
          const parentField = field.additional?.parent;
          
          if (!fieldValue) {
            isValid = false;
          }

          if (parentField) {
            const parentValue = formData[formKey][parentField];

            console.log(parentValue, fieldValue);
            
            if (fieldValue != parentValue) {
              isValid = false;
            }
          }
        });
      }

      return isValid;
    }

    function onReset () {
      for(const formKey in formData){
        for(const formValue in formData[formKey]){
          formData[formKey][formValue] = undefined
        }
      }
    }

    async function onSubmit() {
      if (!validate(props.config)) {
        alert("Ошибка в валидации");
        return;
      }

      try {
        const response = await fetch("fakedata/api", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            parent: toResult(formData.parent),
            child: toResult(formData.child),
          }),
        });
        const result = await response.json();
        console.log(result);
      } catch (error) {
        console.error("Ошибка в отправке формы:", error);
      }
    }

    return {
      formData,
      onSubmit,
      config: props.config,
      components,
      onReset
    };
  },
};
</script>

<style scoped></style>
