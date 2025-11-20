<script setup>
import { cn } from '@/lib/utils'

defineProps({
  class: {
    type: String,
    default: ''
  }
})
</script>

<template>
  <div :class="cn('rounded-lg bg-white text-card-foreground', $props.class)">
    <slot />
  </div>
</template>


