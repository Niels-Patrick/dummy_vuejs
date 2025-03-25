<script setup lang="ts">
    import { ref, onMounted } from 'vue'

    const dummy_text = ref("No text")
    const input_text = ref("")

    function display_text(dum_text: string) {
        dummy_text.value = dum_text
    }

    onMounted(() => {
        console.log(`The initial text is ${dummy_text.value}.`)
    })
</script>

<template>
    <header>
        <h2>Dummy Project Vue</h2>
    </header>

    <body>
        <div>
            <form @submit.prevent="display_text(input_text)">
                <label for="dum_text">Enter text:</label><br><br>
                <input type="text" id="dum_text" name="dum_text" v-model="input_text"><br><br>
                <input type="submit" value="Submit"><br><br>
            </form>
            {{ dummy_text }}
        </div>
    </body>
</template>
