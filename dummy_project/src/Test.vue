<script setup>
  import { ref } from 'vue'
  import { useField, useForm } from 'vee-validate'

  const { handleSubmit, handleReset } = useForm({
    validationSchema: {
      name (value) {
        if (value?.length >= 2) return true

        return 'Name needs to be at least 2 characters.'
      },
      phone (value) {
        if (/^[0-9-]{7,}$/.test(value)) return true

        return 'Phone number needs to be at least 7 digits.'
      },
      email (value) {
        if (/^[a-z.-]+@[a-z.-]+\.[a-z]+$/i.test(value)) return true

        return 'Must be a valid e-mail.'
      },
      select (value) {
        if (value) return true

        return 'Select an item.'
      },
    },
  })
  const name = useField('name')
  const phone = useField('phone')
  const email = useField('email')
  const select = useField('select')

  const items = ref([
    'Item 1',
    'Item 2',
    'Item 3',
    'Item 4',
  ])

  const submit = handleSubmit(values => {
    saveJsonToFile(values)
  })

  function saveJsonToFile(data, filename = "data.json") {
    // Converting JSON object to string
    const jsonStr = JSON.stringify(data, null, 2);

    // Creating a BLOB with JSON data
    const blob = new Blob([jsonStr], { type: "application/json" });

    // Creating a download link
    const a = document.createElement("a");
    a.href = URL.createObjectURL(blob);
    a.download = filename;

    // Appending link to body, trigger click, and remove it
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
  }
</script>

<template>
  <v-container align-center="true">
    <form @submit.prevent="submit">
      <v-text-field
        v-model="name.value.value"
        :counter="10"
        :error-messages="name.errorMessage.value"
        label="Name"
      ></v-text-field>

      <v-text-field
        v-model="phone.value.value"
        :counter="7"
        :error-messages="phone.errorMessage.value"
        label="Phone Number"
      ></v-text-field>

      <v-text-field
        v-model="email.value.value"
        :error-messages="email.errorMessage.value"
        label="E-mail"
      ></v-text-field>

      <v-select
        v-model="select.value.value"
        :error-messages="select.errorMessage.value"
        :items="items"
        label="Select"
      ></v-select>

      <v-btn
        class="me-4"
        type="submit"
      >
        submit
      </v-btn>

      <v-btn @click="handleReset">
        clear
      </v-btn>
    </form>
  </v-container>
</template>
