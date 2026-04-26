<script setup lang='ts'>
import type { FormKitFrameworkContext } from '@formkit/core'
import type { PropType } from 'vue'
import { useFormKitInput } from '../../utils/useFormKitInput'

export interface ListboxItem {
  label?: string
  description?: string
  icon?: string
  avatar?: Record<string, unknown>
  chip?: string | number | Record<string, unknown>
  type?: 'label' | 'separator' | 'item'
  disabled?: boolean
  onSelect?: (event: Event) => void
  class?: unknown
  ui?: Record<string, unknown>
  [key: string]: unknown
}

export interface FormKitListboxProps {
  options?: string[] | ListboxItem[] | ListboxItem[][]
  as?: string
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl'
  by?: string | ((a: unknown, b: unknown) => boolean)
  orientation?: 'horizontal' | 'vertical'
  selectionBehavior?: 'toggle' | 'replace'
  disabled?: boolean
  defaultValue?: unknown
  multiple?: boolean
  valueKey?: string
  labelKey?: string
  descriptionKey?: string
  loading?: boolean
  loadingIcon?: string
  filter?: boolean | Record<string, unknown>
  filterFields?: string[]
  ignoreFilter?: boolean
  selectedIcon?: string
  virtualize?: boolean | {
    overscan?: number
    estimateSize?: number | ((index: number) => number)
  }
  highlight?: boolean
  highlightOnHover?: boolean
  autofocus?: boolean
  autofocusDelay?: number
  searchTerm?: string
  ui?: Record<string, unknown>
}

const props = defineProps({
  context: {
    type: Object as PropType<FormKitFrameworkContext & FormKitListboxProps>,
    required: true,
  },
})

const { handleInput, handleChange, isInvalid, styleClass, modelValue, items, validSlotNames } = useFormKitInput(props.context)
</script>

<template>
  <UListbox
    :id="context.id"
    v-model="modelValue"
    v-bind="{ ...context?.attrs }"
    :class="styleClass"
    :style="context?.attrs.style"
    :as="context.as"
    :size="context.size ?? 'md'"
    :items="items"
    :by="context.by"
    :orientation="context.orientation"
    :selection-behavior="context.selectionBehavior"
    :disabled="!!context?.disabled"
    :default-value="context.defaultValue"
    :multiple="context.multiple"
    :value-key="context.valueKey"
    :label-key="context.labelKey"
    :description-key="context.descriptionKey"
    :loading="context.loading"
    :loading-icon="context.loadingIcon"
    :filter="context.filter"
    :filter-fields="context.filterFields"
    :ignore-filter="context.ignoreFilter"
    :selected-icon="context.selectedIcon"
    :virtualize="context.virtualize"
    :highlight="!!(isInvalid || context.highlight)"
    :highlight-on-hover="context.highlightOnHover"
    :autofocus="context.autofocus"
    :autofocus-delay="context.autofocusDelay"
    :search-term="context.searchTerm"
    :ui="context.ui"
    @change="handleChange"
    @update:model-value="handleInput"
  >
    <template
      v-for="slotName in validSlotNames"
      :key="slotName"
      #[slotName]="slotProps"
    >
      <component
        :is="context?.slots[slotName]"
        v-bind="{ ...context, ...slotProps }"
      />
    </template>
  </UListbox>
</template>

