<template>
    <div class="d-flex flex-column">
        <div class="d-flex justify-content-center mb-4">
            <div class="spinner-border" role="status"></div>
            <!-- <span class="vissually-hidden"> Loading... </span> -->
        </div>
        <p class="text-center">{{ text }}</p>
    </div>
</template>

<script>
export default {
    name: "Spinner",
    props: {
        text: {
            type: String,
        },
    },
};
</script>

<style></style>
