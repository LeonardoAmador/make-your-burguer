<template>
    <div :class="['messageContainer', { 'error': isError }]">
        <p>{{ msg }}</p>
    </div>
</template>

<script>
    export default {
    name: "Message",
    props: {
        msg: String,
        isError: {
        type: Boolean,
        default: false
        }
    }
}
</script>

<style scoped>
.messageContainer {
    color: #004085;
    background-color: #cce5ff;
    border: 2px solid #b8daff;
    border-radius: 5px;
    padding: 10px;
    max-width: 400px;
    margin: 30px auto;
}

.error {
    color: red;
    background-color: #f8d7da;
    border-color: #f5c6cb;
}
</style>
