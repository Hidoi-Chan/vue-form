<template>    
    <label :for="name">
        {{label}}
        <span v-if="required"> *</span>
    </label>
</template>

<script>
export default {
    props: {
        name: String,
        label: String,
        required: Boolean
    }
}
</script>

<style lang="scss" scoped>
    label {
        display: inline-block;
        margin-bottom: 5px;
    }
</style>