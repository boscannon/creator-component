<script setup>
import { ref, watch } from 'vue';
import { ElDatePicker } from 'element-plus';

const props = defineProps({
  modelValue: { type: String },
  placeholder: { type: String },
  disabled: { type: Boolean, default: false },
})
const emit = defineEmits(['update:modelValue'])

const data = ref(props.modelValue);

const emitUpdate = (value) => {
  emit('update:modelValue', value);
};

watch([() => props.modelValue], () => {
  data.value = props.modelValue;
})
</script>

<template>
    <ElDatePicker
      v-model="data"
      value-format="YYYY-MM-DD"
      :placeholder="placeholder"
      class="!w-full"
      :disabled="disabled"
      @change="emitUpdate"
    />
</template>
