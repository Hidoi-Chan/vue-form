<template>
    <section class="accordion" :class="params.isOpen && 'open'">
        <div class="accordion-title">
            <h2>{{params.title}}</h2>
            <div class="icon" />
        </div>
        <div class="accordion-body">
            <slot></slot>
            <div class="error" v-if="params.error.isError">
                <p v-if="params.error.typeOfError === 'date'">Введите корректную дату</p>
                <p v-else-if="params.error.typeOfError === 'phone'">Введите корректный номер телефона</p>
                <p v-else>Заполнены не все обязательные поля секции</p>
            </div>
            <div class="accordion-buttons">
                <Button 
                    v-if="params.buttons.prev" 
                    :left="true"
                    @click="$emit('go-to-prev-section', name)"
                >
                    Назад
                </Button>
                <Button 
                    v-if="params.buttons.next" 
                    :right="true"
                    @click="$emit('check-valid-section', name)"
                >
                    Далее
                </Button>
            </div>
        </div>
    </section>
</template>

<script>
import Button from '@/components/Button'
export default {
    components: {
        Button
    },
    props: {
        params: Object,
        name: String
    }
}
</script>

<style lang="scss" scoped>
    @import "@/scss/variables.scss";

    .accordion-title {
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 20px;
        border-bottom: $border;
        box-sizing: border-box;

        h2 {
            font-size: 1.5rem;
        }

        .icon {
            position: relative;
            width: 30px;
            height: 30px;
            box-sizing: border-box;
            border: $border;
            border-radius: 4px;
            transition: 400ms ease;

            &::after {
                content: '';
                display: block;
                width: 10px;
                height: 10px;
                position: absolute;
                bottom: 5px;
                left: 8px;
                border: solid $grey;
                border-width: 0 2px 2px 0;
                transform: rotate(-135deg);
                transition: 400ms ease;
            }
        }
    }

    .accordion-body {  
        max-height: 0px;
        overflow: hidden;
        padding: 0 20px;
        box-sizing: border-box;
        box-shadow: inset 0 6px 6px -4px rgba(0, 0, 0, .4), inset 0 -6px 6px -4px rgba(0, 0, 0, .4);
        transition: max-height 400ms ease;

        & > .error {
            margin: 10px 0;
            text-align: end;
            font-size: .9rem;
            color: red;
        }
    }

    .accordion.open {

        .accordion-title {

            .icon {
                border: 1px solid #2c3e50;

                &::after {
                    top: 5px;
                    left: 8px;
                    transform: rotate(45deg);
                    border: solid $font;
                    border-width: 0 2px 2px 0;
                }
            }
        }

        .accordion-body {
            max-height: 600px;
            border-bottom: $border;
        }
    }

    .accordion-buttons {
        margin: 15px 0 30px;
        
        &:after {
            content: '';
            display: table;
            clear: both;
        }
    }

</style>