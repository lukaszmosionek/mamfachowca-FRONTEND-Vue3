<template>
  <div class="input-wrap">
    <label v-if="label" class="text-gray-800">{{ label }}</label>
    <input
      :type="type"
      :placeholder="placeholder"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      v-bind="$attrs"
      :class="errors?'border-red-800':''"
      class="w-full px-4 py-2 border border-gray-300 rounded-md text-gray-800 focus:outline-none focus:ring-2 focus:ring-blue-500 mt-1"
    />
    <div v-if="errors" class="text-red-500 mt-1 font-black">
      <span v-for="(msg, i) in errors" :key="i">{{ msg }}</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  label: {
    type: [String, Boolean],
    default: false,
  },
  modelValue: {
    type: String,
    default: "",
  },
  type: {
    type: String,
    default: "text",
  },
  placeholder: {
    type: String,
    default: "",
  },
  errors: {
    type: [String, Array],
    default: () => [],
  },
});

const emit = defineEmits(['update:modelValue']);
</script>

