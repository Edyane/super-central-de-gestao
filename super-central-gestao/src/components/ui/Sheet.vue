<script setup>
import { computed, watch } from 'vue'
import { cn } from '@/lib/utils'

const props = defineProps({
  open: {
    type: Boolean,
    default: false
  },
  side: {
    type: String,
    default: 'right',
    validator: (value) => ['top', 'right', 'bottom', 'left'].includes(value)
  },
  class: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['update:open'])

const isOpen = computed({
  get: () => props.open,
  set: (value) => emit('update:open', value)
})

const sideClasses = {
  top: 'inset-x-0 top-0 border-b',
  right: 'inset-y-0 right-0 h-full w-full border-l sm:max-w-sm',
  bottom: 'inset-x-0 bottom-0 border-t',
  left: 'inset-y-0 left-0 h-full w-full border-r sm:max-w-sm'
}

// Prevent body scroll when sheet is open
watch(isOpen, (newValue) => {
  if (newValue) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
  }
})
</script>

<template>
  <Teleport to="body">
    <Transition name="fade">
      <div
        v-if="isOpen"
        class="fixed inset-0 z-50 bg-black/80"
        @click="isOpen = false"
      />
    </Transition>
    <Transition :name="side === 'left' || side === 'right' ? 'slide-x' : 'slide-y'">
      <div
        v-if="isOpen"
        :class="cn(
          'fixed z-50 bg-background p-6 shadow-lg transition-transform',
          sideClasses[side],
          props.class
        )"
      >
        <slot />
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-x-enter-active,
.slide-x-leave-active {
  transition: transform 0.3s ease;
}

.slide-x-enter-from {
  transform: translateX(100%);
}

.slide-x-leave-to {
  transform: translateX(100%);
}

.slide-y-enter-active,
.slide-y-leave-active {
  transition: transform 0.3s ease;
}

.slide-y-enter-from {
  transform: translateY(100%);
}

.slide-y-leave-to {
  transform: translateY(100%);
}
</style>

