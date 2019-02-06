<template functional>
    <label class="text-muted small font-weight-bold" :for="props.for">
        {{ props.text }}
    </label>
</template>

<script>
    export default {
        name: 'Label',
        functional: true,
        props: {
            text: {
                type: String,
                required: true
            },
            for: {
                type: String,
                required: true
            }
        }
    }
</script>