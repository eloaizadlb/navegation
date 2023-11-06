<template>
    <div :class="['chip', `chip-${type}`]">
        {{ text }}
    </div>
</template>

<script>
export default {
    name: 'ChipUI',
    props: {
        text: {
            type: String,
            required: true
        },
        type: {
            type: String,
            default: 'default'
        }
    }
}
</script>

<style scoped>
.chip {
    display: inline-block;
    padding: 0.25rem 0.5rem;
    border-radius: 9999px;
    font-size: 0.875rem;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.05em;
}

.chip-default {
    background-color: #e0e0e0;
    color: #333;
}

.chip-primary {
    background-color: #1976d2;
    color: #fff;
}

.chip-secondary {
    background-color: #f50057;
    color: #fff;
}

.chip-success {
    background-color: #4caf50;
    color: #fff;
}

.chip-warning {
    background-color: #ff9800;
    color: #fff;
}

.chip-error {
    background-color: #f44336;
    color: #fff;
}
</style>
