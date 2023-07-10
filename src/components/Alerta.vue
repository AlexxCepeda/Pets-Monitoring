<template>
  <div
    :class="[isError ? 'bg-red-500': 'bg-green-500']"
    class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md mt-4">
    {{ alerta.mensaje }}
  </div>
</template>

<script setup>
import { computed } from 'vue';
const props = defineProps({
    alerta: {
        type: Object,
        required: true
    }
})

const isError = computed(() => props.alerta.tipo === 'error' )

</script>

