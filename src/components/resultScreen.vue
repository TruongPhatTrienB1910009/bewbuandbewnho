<template>
    <div class="screen">
        <h1>Congratulations</h1>
        <p>{{ Math.round(timer / 1000) }} seconds</p>
        <button @onclick="onStartAgain">Start Again</button>
    </div>
</template>

<script>
export default {
    props: {
        timer: {
            type: Number,
            required: true,
        }
    },
    methods: {
        onStartAgain() {
            this.$emit("onStartAgain");
        }
    },
}
</script>
