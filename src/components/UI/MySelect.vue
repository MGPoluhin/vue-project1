<template>
  <select
      v-model="modelValue"
      @change="cnangeOption"
  >
    <option disabled value="">Выберите из списка</option>
    <option
        v-for="option in options"
        :key="options.value"
        :value="options.value"
    >
      {{option.name}}
    </option>
  </select>
</template>

<script>
export default {
  name: "my-select",
  props:{
    modelValue: {
      type: String
    },
    options:{
      type: Array,
      default: () => []
    }
  },
  methods: {
    cnangeOption(event) {
      console.log(event)
      console.log(event.target.value)
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<style scoped>

</style>