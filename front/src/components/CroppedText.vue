<template>
    {{ croppedText }}
</template>

<script setup>
import { ref, onMounted } from 'vue';
const props = defineProps({
    text: String,
    maxLength: {
        type: Number,
        default: 100,
    },
});
const croppedText = ref('');

onMounted(() => {
    croppedText.value = props.text.length > props.maxLength
        ? `${props.text.slice(0, props.maxLength)}...`
        : props.text;
});
</script>