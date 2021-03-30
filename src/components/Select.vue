<template>
    <div class="select-block" :class="{multiple, error}">
        <Input 
            :name="name"
            :readonly="true"
            :sectionKey="sectionKey"
            :value="value"
            :open="open"
            @blur="closeSelect"
            @click="toggleSelect"
        />
        <span></span>
        <div class="options-container" :class="{open}" ref="options">
            <ul class="options">
                <li v-for="option in options" :key="option.value" class="option">
                    <input 
                        :type="multiple ? 'checkbox' : 'radio'"
                        @change="$emit('change', sectionKey, name, $event.target.value)"
                        name="option.name"
                        :id="option.value" 
                        :value="option.label"
                    />
                    <label :for="option.value">
                        {{option.label}}
                    </label>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import Input from '@/components/Input'
export default {
    props: {
        name: String,
        multiple: Boolean,
        options: Array,
        open: Boolean,
        sectionKey: String,
        value: String,
        error: Boolean
    },
    methods: {
        toggleSelect(sectionKey, name) {
            this.$emit('toggle-select', sectionKey, name)
        },
        closeSelect(sectionKey, name, target) {
            this.$emit('close-select', sectionKey, name, target)
        }
    },
    components: {
        Input
    }
}
</script>

<style lang="scss" scoped>
    @import "@/scss/variables.scss";

    .select-block {
        position: relative;
    }

    input {
        padding-right: 36px;

        & + span {
            display: block;
            position: absolute;
            top: 10px;
            right: 12px;
            width: 10px;
            height: 10px;
            box-sizing: border-box;
            border: solid #888;
            border-width: 0 2px 2px 0;
            transform: rotate(45deg);
            pointer-events: none;
            transition: border 200ms ease;
        }

        &:hover,
        &:focus,
        &.focus {
            & + span {
                border: solid $darkblue;
                border-width: 0 2px 2px 0;
            }
        }

        &:focus,
        &.focus {
            border: 1px solid $darkblue;
            border-radius: 4px 4px 0 0;
            box-shadow: 0 0 5px $darkblue7;
        }
    }

    .select-block.error {

        input {
            border: 1px solid $error;

            &:hover,
            &:focus,
            &.focus {
                box-shadow: 0 0 5px $error;
            }
            
            & + span {
                border: solid $error;
                border-width: 0 2px 2px 0;
            }
        }
    }

    .options-container {
        max-height: 0;
        overflow: hidden;
        position: absolute;
        width: 100%;
        transition: max-height 400ms ease;
    }

    .options-container.open {
        max-height: 200px;
    }

    .options {
        width: 100%;
        border: 1px solid $darkblue;
        border-top: none;
        border-radius: 0 0 4px 4px;
        box-sizing: border-box;
        background: #fff;
    }

    .option {
        display: block;
        width: 100%;
        transition: background 200ms ease;

        label {
            display: block;
            padding: 6px 16px;
            
            &:hover {            
                cursor: pointer;
                background: $blue2;
            }
        }
    }
        
    input[type="radio"],
    input[type="checkbox"] {
        display: none;
    }

    input[type="radio"]:checked + label {
        background: $blue5;                
    }

    .select-block.multiple {
        input[type="checkbox"] + label {
            position: relative;
        }

        input[type="checkbox"] + label::before {
            content: '';
            display: block;
            width: 20px;
            height: 20px;
            position: absolute;
            right: 16px;
            top: calc(50% - 10px);
            border: $border;
            border-radius: 4px;
            box-sizing: border-box;
            transition: all 200ms ease;
        }

        input[type="checkbox"] + label:hover::before {
            border: 1px solid $darkblue;
            box-shadow: 0 0 5px $darkblue7;
        }

        input[type="checkbox"]:checked + label::before {
            border: 1px solid $darkblue;
            background: $darkblue;
        }

        input[type="checkbox"] + label::after {
            content: '';
            position: absolute;
            width: 6px;
            height: 10px;
            top: 9px;
            right: 23px;
            box-sizing: border-box;
            border: solid #fff;
            border-width: 0 2px 2px 0;
            transform: rotate(45deg);
            opacity: 0;
            transition: opacity 200ms ease;
        }

        input[type="checkbox"] + label::after {
            opacity: 1;
        }
    }
</style>