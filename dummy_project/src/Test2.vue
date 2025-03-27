<!--
    Adds a small form to enter some text and displays it
    in an alert box when submitted
-->
<script setup>
    import { ref } from 'vue'

    const someText = ref('')

    const rules = [
        value => {
            if (value) return true
            return 'You must enter a text.'
        },
    ]

        function display_text() {
        if (someText.value != '') {
            alert(someText.value)
        }
    }
</script>

<template>
    <v-card title="Please enter some text" class="pa-2 ma-2" width="20%">
        <v-container>
            <v-form @submit.prevent>
                <v-text-field
                    v-model="someText"
                    color="orange-lighten-1"
                    variant="outlined"
                    :rules="rules"
                    label="Enter some text"
                    ></v-text-field>

                <v-btn
                    class="mt-2"
                    type="submit"
                    block
                    color="blue-darken-1"
                    variant="elevated"
                    @click="display_text"
                >
                    Submit
                </v-btn>
            </v-form>
        </v-container>
    </v-card>
</template>
