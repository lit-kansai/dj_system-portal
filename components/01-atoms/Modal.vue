<template>
  <div
    v-if="opened"
    class="fixed overflow-y-auto top-0 bottom-0 left-0 right-0 z-20 bg-black animate-fade-in-down transform bg-opacity-40 pt-10 pb-10"
    @click="close()"
  >
    <div class="flex items-center justify-center">
      <div
        class="w-[95%] max-w-sm md:max-w-6xl relative z-30 m-6 p-3.5 md:p-5 border-2 opacity-100 rounded-2xl border-neon-blue shadow-modal-neon-blue"
      >
        <ModalCloseButton
          class="absolute top-0 right-0 transform origin-center translate-x-1/2 -translate-y-1/2"
          @click.native="close()"
        />
        <div
          class="border-2 border-neon-pink shadow-modal-neon-pink rounded-2xl"
          @click.stop=""
        >
          <div class="bg-gray-800 rounded-2xl">
            <slot name="content"> </slot>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  props: {
    opened: { type: Boolean, required: true },
  },
  emits: ['close'],
  setup(_, { emit }) {
    const close = (): void => {
      emit('close')
    }
    return { close }
  },
})
</script>
