<script setup>
  import { ref } from 'vue'
  import { useField, useForm } from 'vee-validate'
  import Test2 from './Test2.vue'

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

  const { handleSubmit2, handleReset2 } = useForm({
    validationSchema: {
      name2 (value) {
        if (value?.length >= 2) return true

        return 'Name needs to be at least 2 characters.'
      },
      calories (value) {
        if (/^[0-9-]/.test(value)) return true

        return 'Please enter numbers only.'
      },
    },
  })

  const name2 = useField('name2')
  const calories = useField('calories')

  const submit2 = handleSubmit2(values => {
    addDessert(values)
  })

  const newDessert = ref({ name: "", calories: null })

  function addDessert(data) {
    if(data.value.name && data.value.calories) {
      desserts.value.push({ ...data.value }) // Adding to list
      newDessert.value = { name: "", calories: null } // Reseting form
    }
  }

  const desserts = ref([
      {
        name: 'Frozen Yogurt',
        calories: 159,
      },
      {
        name: 'Ice cream sandwich',
        calories: 237,
      },
      {
        name: 'Eclair',
        calories: 262,
      },
      {
        name: 'Cupcake',
        calories: 305,
      },
      {
        name: 'Gingerbread',
        calories: 356,
      },
      {
        name: 'Jelly bean',
        calories: 375,
      },
      {
        name: 'Lollipop',
        calories: 392,
      },
      {
        name: 'Honeycomb',
        calories: 408,
      },
      {
        name: 'Donut',
        calories: 452,
      },
      {
        name: 'KitKat',
        calories: 518,
      },
    ])
</script>

<template>
  <v-card class="pa-4 ma-4" color="blue-lighten-5">
    <v-layout>
      <v-app-bar :elevation="2" color="blue-darken-2">
        <v-app-bar-title>Dummy Project</v-app-bar-title>
      </v-app-bar>
    </v-layout>

    <v-layout>
      <v-main>
        <v-row>
          <v-col>
            <v-card title="Please fill the form" class="pa-2 ma-2">
              <v-container>
                <form @submit.prevent="submit">
                  <v-text-field
                    v-model="name.value.value"
                    color="orange-lighten-1"
                    variant="outlined"
                    :counter="10"
                    :error-messages="name.errorMessage.value"
                    label="Name"
                  ></v-text-field>

                  <v-text-field
                    v-model="phone.value.value"
                    color="orange-lighten-1"
                    variant="outlined"
                    :counter="7"
                    :error-messages="phone.errorMessage.value"
                    label="Phone Number"
                  ></v-text-field>

                  <v-text-field
                    v-model="email.value.value"
                    color="orange-lighten-1"
                    variant="outlined"
                    :error-messages="email.errorMessage.value"
                    label="E-mail"
                  ></v-text-field>

                  <v-select
                    v-model="select.value.value"
                    color="orange-lighten-1"
                    variant="outlined"
                    :error-messages="select.errorMessage.value"
                    :items="items"
                    label="Select"
                  ></v-select>

                  <v-btn
                    class="me-4"
                    type="submit"
                    color="blue-darken-1"
                    variant="elevated"
                  >
                    submit
                  </v-btn>

                  <v-btn
                    @click="handleReset"
                    color="red-darken-1"
                    variant="elevated"
                  >
                    clear
                  </v-btn>
                </form>
              </v-container>
            </v-card>
          </v-col>

          <v-col>
            <v-card class="pa-2 ma-2">
              <v-table density="compact">
                <thead>
                  <tr>
                    <th class="text-left">
                      Name
                    </th>
                    <th class="text-left">
                      Calories
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="item in desserts"
                    :key="item.name"
                  >
                    <td>{{ item.name }}</td>
                    <td>{{ item.calories }}</td>
                  </tr>
                </tbody>
              </v-table>
            </v-card>
          </v-col>
        </v-row>
      </v-main>
    </v-layout>
    <v-divider
      :thickness="5"
      color="orange-lighten-4"
      class="border-opacity-100 ma-2"
    ></v-divider>
    <v-layout>
      <v-main>
        <v-row>
          <v-col>
            <Test2 />
          </v-col>

          <v-col>
            <!--
            <v-card title="Please fill the form" class="pa-2 ma-2" width="50%">
              <v-container>
                <form @submit.prevent="submit2">
                  <v-text-field
                    v-model="name2.value.value"
                    color="orange-lighten-1"
                    variant="outlined"
                    :counter="10"
                    :error-messages="name2.errorMessage.value"
                    label="Name"
                  ></v-text-field>

                  <v-text-field
                    v-model="calories.value.value"
                    color="orange-lighten-1"
                    variant="outlined"
                    :counter="7"
                    :error-messages="calories.errorMessage.value"
                    label="Calories"
                  ></v-text-field>

                  <v-btn
                    class="me-4"
                    type="submit"
                    color="blue-darken-1"
                    variant="elevated"
                  >
                    submit
                  </v-btn>

                  <v-btn
                    @click="handleReset"
                    color="red-darken-1"
                    variant="elevated"
                  >
                    clear
                  </v-btn>
                </form>
              </v-container>
            </v-card> -->
          </v-col>
        </v-row>
      </v-main>
    </v-layout>
  </v-card>
</template>
