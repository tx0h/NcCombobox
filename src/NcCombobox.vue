<script setup>
import { computed } from 'vue'
import NcSelect from '@nextcloud/vue/components/NcSelect'

/**
 * NcCombobox - A Nextcloud-styled input with real-time suggestions.
 * Follows the Nextcloud Design System (NDS).
 */
const props = defineProps({
	/** @values Array of strings or objects {label: string, value: any} */
	options: { type: Array, required: true },
	/** The floating label for the input */
	label: { type: String, default: '' },
	/** Placeholder text when input is empty */
	placeholder: { type: String, default: '' },
	/** The current value (v-model) */
	modelValue: { type: String, default: '' },
	/** Direction of the dropdown: 'top', 'bottom', or 'auto' */
	openDirection: { type: String, default: 'auto' },
	/** Whether to append the dropdown to the body to avoid overflow issues */
	appendToBody: { type: Boolean, default: false }
})

const emit = defineEmits(['update:modelValue'])

/**
 * Handle value synchronization. 
 * Converts between NcSelect's object-based selection and our string-based v-model.
 */
const internalValue = computed({
	get() {
		return props.modelValue || ''
	},
	set(newValue) {
		const val = typeof newValue === 'object' && newValue !== null 
			? (newValue.label || newValue.value) 
			: newValue
		emit('update:modelValue', val || '')
	}
})

/**
 * Standardize options for NcSelect internal processing.
 */
const formattedOptions = computed(() => {
	return props.options.map(opt => 
		typeof opt === 'object' ? opt : { label: opt, value: opt }
	)
})

/**
 * Emits the search string immediately on every keystroke.
 * This is the core 'combobox' functionality.
 */
const onSearchChange = (search) => {
	emit('update:modelValue', search)
}
</script>

<template>
	<div class="nc-combobox">
		<NcSelect
			v-model="internalValue"
			:options="formattedOptions"
			:input-label="label"
			:placeholder="placeholder"
			:taggable="true"
			:push-tags="true"
			:filterable="true"
			:close-on-select="true"
			:open-direction="openDirection"
			:append-to-body="appendToBody"
			@search-change="onSearchChange"
		>
			<!-- Slot for custom 'no options' message -->
			<template #no-options>
				<slot name="no-options">
					{{ placeholder || 'Tippen zum Suchen...' }}
				</slot>
			</template>
			
			<!-- Forwarding other slots if needed -->
			<template v-for="(_, name) in $slots" #[name]="slotProps">
				<slot :name="name" v-bind="slotProps || {}" />
			</template>
		</NcSelect>
	</div>
</template>

<style scoped>
.nc-combobox {
	width: 100%;
	margin-bottom: var(--nc-margin-bottom, 12px);
}

/* Ensure border colors stay in sync with global themes */
:deep(.multiselect__tags) {
	border-color: var(--color-border);
	transition: border-color var(--animation-quick);
}

:deep(.multiselect--active .multiselect__tags) {
	border-color: var(--color-primary-element);
}
</style>
