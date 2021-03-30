<template>
    <button 
        class="button" 
        :class="{right, left}"
        @click.prevent="$emit('click')"
        :disabled="disabled"
    >
        <slot></slot>
    </button>
</template>

<script>
export default {
    props: {
        right: Boolean,
        left: Boolean,
        disabled: Boolean
    }
}
</script>

<style lang="scss" scoped>
    @import "@/scss/variables.scss";

    .button {
        border-radius: 4px;
        padding: 8px 20px;
        background: $darkblue;
        font-size: 1rem;
        color: #fff;
        transition: all 200ms ease;

        &:hover {
            cursor: pointer;
            box-shadow: 0 0 5px $darkblue;
        }

        &.right {
            float: right;
        }

        &.left {
            float: left;
            background: $darkblue7;
        }

        &:disabled {
            background: $grey;

            &:hover {
                cursor: auto;
                box-shadow: none;
            }
        }
    }
</style>