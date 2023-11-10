<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <div class="max-w-md w-full bg-white p-8 rounded-md shadow-md">
      <form @submit.prevent="submitForm">
        <div class="mb-4">
          <label for="username" class="block text-sm font-medium text-gray-600">Username:</label>
          <input
            id="username"
            v-model="formData.username"
            class="mt-1 p-2 w-full border rounded-md"
          />
          <div v-for="error in $v.username.$errors" :key="error.$uid">
            <div class="text-red-600 text-sm">{{ error.$message }}</div>
          </div>
        </div>
        <div class="mb-4">
          <label for="email" class="block text-sm font-medium text-gray-600">Email:</label>
          <input
            id="email"
            v-model="formData.email"
            class="mt-1 p-2 w-full border rounded-md"
          />
          <div v-for="error in $v.email.$errors" :key="error.$uid">
            <div class="text-red-600 text-sm">{{ error.$message }}</div>
          </div>
        </div>
        <button
          type="submit"
          class="px-4 py-2 text-white bg-blue-500 rounded-md hover:bg-blue-600"
        >
          Submit
        </button>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { required, minLength, email } from "@vuelidate/validators";

const formData = ref({
  username: "",
  email: "",
});

const rules = {
  username: { required, minLength: minLength(3), $autoDirty: true },
  email: { required, email, $autoDirty: true },
};

const $v = useVuelidate(rules, formData);

function submitForm() {
  $v.value.$touch();
  if ($v.value.$invalid) {
    console.log("Form is invalid!");
  } else {
    console.log("Form data:", formData.value);
  }
}
</script>
