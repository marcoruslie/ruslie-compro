<template>
	<div class="flex flex-col">
		<label class="font-semibold text-gray-700 flex items-center gap-2 text-sm md:text-base">
			<i :class="'fa-solid fa-' + icon + ' text-blue-500 text-base md:text-lg'"></i>
			{{ label }}
		</label>

		<input :value="modelValue" @input="emit('update:modelValue', parseFloat($event.target.value) || null)"
			type="number" :step="step || 1" :placeholder="placeholder" class="mt-2 w-full border border-gray-300 rounded-lg md:rounded-xl
             p-2 md:p-3 text-sm md:text-base bg-white text-gray-700
             focus:ring-2 focus:ring-blue-400 focus:border-blue-400
             transition-all duration-200 hover:shadow-sm md:hover:shadow-md" />
	</div>
</template>

<script setup>
const props = defineProps({
	label: String,
	icon: String,
	modelValue: [Number, String, null],
	placeholder: String,
	step: [Number, String],
})

const emit = defineEmits(['update:modelValue'])
</script>
