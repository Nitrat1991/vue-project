<template>
    <input :value="modelValue" @input="updateInput" class="input" type="text">
</template>

<script>
    export default {
      name: 'my-input',
      props: {
        modelValue: [String, Number]
      },
      methods: {
        updateInput(event) {
            this.$emit('update:modelValue', event.target.value)
        } 
      }
    }
</script>

<style scope>
.input {
    border: 2px solid teal;
    padding: 18px 15px;
    margin-top: 10px;
}
</style>