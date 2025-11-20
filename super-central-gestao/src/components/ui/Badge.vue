<script setup>
import { cn } from '@/lib/utils'

const props = defineProps({
  variant: {
    type: String,
    default: 'default',
    validator: (value) => ['default', 'secondary', 'destructive', 'outline'].includes(value)
  },
  class: {
    type: String,
    default: ''
  }
})

const variantClasses = {
  default: 'bg-primary text-primary-foreground',
  secondary: 'bg-secondary text-secondary-foreground',
  destructive: 'bg-destructive text-destructive-foreground',
  outline: 'border border-input bg-background'
}
</script>

<template>
  <div :class="cn(
    'inline-flex items-center rounded-full border px-2.5 py-0.5 text-xs font-semibold transition-colors',
    variantClasses[variant],
    props.class
  )">
    <slot />
  </div>
</template>


