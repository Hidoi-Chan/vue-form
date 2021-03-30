<template>
    <input 
        type="text" 
        :id="name" 
        :readonly="readonly"
        :value="value"
        :class="{focus: open}"
        :placeholder="placeholder"
        @input="$emit('input', $event.target.value, sectionKey, name)"
        @focus="$emit('focus', sectionKey, name)"
        @blur="$emit('blur', sectionKey, name, $event.target)"
        @click="$emit('click', sectionKey, name)"
    />
</template>

<script>
export default {
    props: {
        name: String,
        readonly: Boolean,
        value: String,
        touched: Boolean,
        sectionKey: String,
        open: Boolean,
        placeholder: String
    }
}
</script>

<style lang="scss" scoped>
    @import "@/scss/variables.scss";

    input {
        width: 100%;
        border: $border;
        border-radius: 4px;
        padding: 8px 16px;
        box-sizing: border-box;
        font-size: 0.9rem;
        line-height: 1rem;
        transition: all 200ms ease;

        &:hover,
        &:focus {
            border: 1px solid $darkblue;
            box-shadow: 0 0 5px $darkblue7;
        }

        &[readonly] {
            cursor: pointer;
        }
    }
</style>