<template>
    <form class="form">        
        <div class="form-header">
            <h1>Форма регистрации</h1>
        </div>
        <div class="form-body">
            <Section 
                v-for="(value, key) in sections" 
                :key="key" 
                :name="key"
                :params="value"
                @check-valid-section="checkValidSection"
                @go-to-prev-section="goToPrevSection"
            >
                <div class="grid" :class="value.addClass && [key]">
                    <div 
                        v-for="(elem, elemKey) in formdata[key]" 
                        :key="elemKey" 
                        :class="{
                            [elemKey]: elem.addClass, 
                            error: (!elem.value.trim() && elem.touched && !!elem.validation) || elem.error
                        }"
                    >
                        <Label 
                            :name="elemKey"
                            :label="elem.label"
                            :required="elem.validation ? elem.validation.required : false"
                        />
                        <Input 
                            v-if="elem.type === 'input'"
                            :name="elemKey"
                            :placeholder="elem.placeholder"
                            :sectionKey="key"
                            :touched="elem.touched"
                            v-model="elem.value"
                            @input="changeInput"
                            @focus="focusInput"
                            @blur="blurInput"
                        />
                        <Select 
                            v-else-if="elem.type === 'select'"
                            :name="elemKey"
                            :sectionKey="key"
                            :multiple="elem.multiple"
                            :value="elem.value"
                            :open="elem.isOpen"
                            :options="elem.options"
                            :error="!elem.value.trim() && elem.touched && !!elem.validation"
                            @change="changeSelect"
                            @toggle-select="toggleSelect"
                            @close-select="closeSelect"
                        />
                    </div>
                </div>
            </Section>
        </div>
        <div class="form-footer">
            <div class="success" v-if="isValid">Новый клиент успешно создан</div>
            <div class="notifications" v-else>
                <input type="checkbox" id="notifications" v-model="notification.value">
                <label class="notifications" for="notifications">{{notification.label}}</label>
            </div>
            <Button @click="sendForm">Зарегистрироваться</Button>
        </div>
    </form>
</template>

<script>
import Section from '@/components/Section'
import Label from '@/components/Label'
import Input from '@/components/Input'
import Select from '@/components/Select'
import Button from '@/components/Button'
export default {
    data: () => ({
        sections: {
            person: {
                title: 'Персональные данные',
                isOpen: true,
                isValid: false,
                error: {
                    isError: false,
                    typeOfError: ''
                },
                buttons: {
                    prev: false,
                    next: true
                }
            },
            adress: {
                title: 'Адресные данные',
                isOpen: false,
                isValid: false,
                error: {
                    isError: false,
                    typeOfError: ''
                },
                buttons: {
                    prev: true,
                    next: true
                }
            },
            document: {
                title: 'Удостоверение личности',
                isOpen: false,
                isValid: false,
                error: {
                    isError: false,
                    typeOfError: ''
                },
                buttons: {
                    prev: true,
                    next: true
                }
            },
            client: {
                title: 'Параметры клиента',
                isOpen: false,
                isValid: false,
                error: {
                    isError: false,
                    typeOfError: ''
                },
                buttons: {
                    prev: true,
                    next: false
                },
                addClass: true
            }
        },
        formdata: {
            person: {
                lastname: {
                    type: 'input',
                    label: 'Фамилия',
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    validation: {
                        required: true
                    }
                },
                firstname: {
                    type: 'input',
                    label: 'Имя',
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    validation: {
                        required: true
                    }
                },
                patronymic: {
                    type: 'input',
                    label: 'Отчество',
                    value: ''
                },
                gender: {
                    type: 'select',
                    multiple: false,
                    label: 'Пол',
                    isOpen: false,
                    options: [
                        {
                            name: 'genderRadio',
                            value: 'male',
                            label: 'Мужчина'
                        },
                        {
                            name: 'genderRadio',
                            value: 'female',
                            label: 'Женщина'
                        }
                    ],
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    validation: {
                        required: true
                    }
                },
                'date-of-birth': {
                    type: 'input',
                    label: 'Дата рождения',
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    placeholder: 'ДД.ММ.ГГГГ',
                    validation: {
                        required: true,
                        data: 'date'
                    }
                },
                phone: {
                    type: 'input',
                    label: 'Номер телефона',
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    placeholder: '7 (999) 999-99-99',
                    validation: {
                        required: true,
                        data: 'phone'
                    }
                }
            },
            adress: {
                country: {
                    type: 'input',
                    label: 'Страна',
                    value: ''
                },
                region: {
                    type: 'input',
                    label: 'Область',
                    value: ''
                },
                city: {
                    type: 'input',
                    label: 'Город',
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    validation: {
                        required: true
                    }
                },
                index: {
                    type: 'input',
                    label: 'Индекс',
                    value: ''
                },
                street: {
                    type: 'input',
                    label: 'Улица',
                    value: ''
                },
                house: {
                    type: 'input',
                    label: 'Дом',
                    value: ''
                }
            },
            document: {
                'type-of-document': {
                    type: 'select',
                    multiple: false,
                    label: 'Тип документа',
                    isOpen: false,
                    options: [
                        {
                            name: 'document',
                            value: 'passport',
                            label: 'Паспорт'
                        },
                        {
                            name: 'document',
                            value: 'birth-certificate',
                            label: 'Свидетельтво о рождении'
                        },
                        {
                            name: 'document',
                            value: 'drivers-license',
                            label: 'Водительское удостоверение'
                        }
                    ],
                    addClass: true,
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    validation: {
                        required: true
                    }
                },
                series: {
                    type: 'input',
                    label: 'Серия',
                    value: ''
                },
                number: {
                    type: 'input',
                    label: 'Номер',
                    value: ''
                },
                'date-of-issue': {
                    type: 'input',
                    label: 'Дата выдачи',
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    placeholder: 'ДД.ММ.ГГГГ',
                    validation: {
                        required: true,
                        data: 'date'
                    }
                },
                'issued-by': {
                    type: 'input',
                    label: 'Кем выдан',
                    addClass: true,
                    value: ''
                }
            },
            client: {
                doctor: {
                    type: 'select',
                    multiple: false,
                    label: 'Лечащий врач',
                    isOpen: false,
                    options: [
                        {
                            name: 'doctorName',
                            value: 'Ivanov',
                            label: 'Иванов'
                        },
                        {
                            name: 'doctorName',
                            value: 'Zaharov',
                            label: 'Захаров'
                        },
                        {
                            name: 'doctorName',
                            value: 'Chernisheva',
                            label: 'Чернышева'
                        }
                    ],
                    value: ''
                },
                'type-of-client': {
                    type: 'select',
                    multiple: true,
                    label: 'Тип клиента',
                    isOpen: false,
                    options: [
                        {
                            name: 'type-of-client-checkbox',
                            value: 'vip',
                            label: 'VIP'
                        },
                        {
                            name: 'type-of-client-checkbox',
                            value: 'problematic',
                            label: 'Проблемные'
                        },
                        {
                            name: 'type-of-client-checkbox',
                            value: 'health-insurance',
                            label: 'ОМС'
                        }
                    ],
                    value: '',
                    touched: false,
                    valid: false,
                    error: '',
                    validation: {
                        required: true
                    }
                }
            }
        },
        notification: {
            label: 'Уведомлять по СМС',
            value: true
        },
        isValid: false
    }),
    methods: {
        focusInput(sectionKey, formdataKey) {
            if (!!this.formdata[sectionKey][formdataKey].validation) {
                this.formdata[sectionKey][formdataKey].touched = false
            }
        },
        blurInput(sectionKey, formdataKey) {
            if (!!this.formdata[sectionKey][formdataKey].validation) {
                this.formdata[sectionKey][formdataKey].touched = true

                if (this.formdata[sectionKey][formdataKey].validation.data === 'phone') {
                    this.renderPhoneNumber(sectionKey, formdataKey)
                }  

                if (this.formdata[sectionKey][formdataKey].validation.data === 'date') {
                    this.renderDate(sectionKey, formdataKey)
                }  
            }
        },
        renderPhoneNumber(sectionKey, formdataKey) {
            const value = this.formdata[sectionKey][formdataKey].value
            const arr = value.split('').filter(item => /\d/.test(item))
            if (arr.length === 11 && (arr[0] === '8' || arr[0] === '7')) {
                arr[0] = '7'
                arr.splice(1, 0, ' (')
                arr.splice(5, 0, ') ')
                arr.splice(9, 0, '-')
                arr.splice(12, 0, '-')
                this.formdata[sectionKey][formdataKey].value = arr.join('')
            } else if (!this.formdata[sectionKey][formdataKey].error) {
                this.formdata[sectionKey][formdataKey].error = 'phone'
            }
        },
        renderDate(sectionKey, formdataKey) {
            let value = this.formdata[sectionKey][formdataKey].value
            const arr = value.split(/\W/).filter(item => /\d/.test(item)).map(item => +item)
            const now = new Date()
            const date = new Date(arr[2], arr[1] - 1, arr[0])
            if (arr.length === 3 && arr[0] <= 31 && arr[1] <= 12 && (arr[2] <= 99 || arr[2] > 1900 ) && date.getTime() <= now.getTime()) {
                const day = ('0' + date.getDate()).slice(-2)
                const month = ('0' + (date.getMonth() + 1)).slice(-2)
                const year = date.getFullYear()
                value = `${day}.${month}.${year}`
                this.formdata[sectionKey][formdataKey].value = value
            } else if (!this.formdata[sectionKey][formdataKey].error) {
                this.formdata[sectionKey][formdataKey].error = 'date'
            }
        },
        changeInput(value, sectionKey, formdataKey) {
            if (!!this.formdata[sectionKey][formdataKey].validation) {
                if (value.trim()) {
                    this.formdata[sectionKey][formdataKey].valid = true
                    this.formdata[sectionKey][formdataKey].error = ''
                } else {
                    this.formdata[sectionKey][formdataKey].valid = false
                    this.formdata[sectionKey][formdataKey].error = 'required'
                }
            }
        },
        checkValidSection(sectionName) {
            let result = true
            this.sections[sectionName].error.isError = false
            this.sections[sectionName].error.typeOfError = ''

            Object.keys(this.formdata[sectionName]).map(key => {
                if (!!this.formdata[sectionName][key].validation === true) {
                    if (this.formdata[sectionName][key].error ||
                        !this.formdata[sectionName][key].valid
                    ) {
                        this.sections[sectionName].error.isError = true
                        this.formdata[sectionName][key].valid = false
                        this.formdata[sectionName][key].touched = true

                        if (this.sections[sectionName].error.typeOfError !== 'required') {
                            this.sections[sectionName].error.typeOfError = this.formdata[sectionName][key].error
                        }
                        result = false
                    } 
                }               
            })

            this.sections[sectionName].isValid = result
            if (this.sections[sectionName].isValid) {
                this.sections[sectionName].error.isError = false
            }
        },
        goToPrevSection(sectionName) {
            this.sectionsKeys.map((key, index) => {
                if (key === sectionName) {
                    this.sections[key].isOpen = false
                    this.sections[this.sectionsKeys[index - 1]].isValid = false
                    this.sections[this.sectionsKeys[index - 1]].isOpen = true
                }
            })
        },
        toggleSelect(sectionKey, formdataKey) {
            if (this.formdata[sectionKey][formdataKey].isOpen) {
                this.formdata[sectionKey][formdataKey].isOpen = false
            } else {
                this.formdata[sectionKey][formdataKey].isOpen = true
            }

            this.formdata[sectionKey][formdataKey].touched = false
        },
        closeSelect(sectionKey, formdataKey, target) {            
            if (!!this.formdata[sectionKey][formdataKey].validation) {
                this.formdata[sectionKey][formdataKey].touched = true
                this.changeInput(
                    this.formdata[sectionKey][formdataKey].value,
                    sectionKey,
                    formdataKey
                )
            }
            
            if (this.formdata[sectionKey][formdataKey].isOpen) {
                this.formdata[sectionKey][formdataKey].touched = false
                target.focus()
            }
        },
        changeSelect(sectionKey, formdataKey, value) {
            if (!this.formdata[sectionKey][formdataKey].multiple) {
                this.formdata[sectionKey][formdataKey].isOpen = false
                this.formdata[sectionKey][formdataKey].value = value
            } else {
                let arr = []
                if (this.formdata[sectionKey][formdataKey].value) {
                    arr = this.formdata[sectionKey][formdataKey].value.split(', ')
                }
                const index = arr.indexOf(value)
                if (index === -1) {
                    arr.push(value)
                } else {
                    arr.splice(index, 1)
                }
                this.formdata[sectionKey][formdataKey].value = arr.join(', ')
            }
        },
        sendForm() {
            let result = true
            this.sectionsKeys.map((key, index) => {
                if (index === this.sectionsKeys.length - 1 && result === true) {
                    this.checkValidSection(key)
                }

                if (!this.sections[key].isValid) {
                    result = false
                }
            })

            this.isValid = result
        }
    },
    watch: {
        sections: {
            handler(sections) {
                this.sectionsKeys.map((key, index) => {
                    if (sections[key].isOpen && sections[key].isValid) {
                        sections[key].isOpen = false
                        if (index + 1 < this.sectionsKeys.length) {
                            sections[this.sectionsKeys[index + 1]].isOpen = true
                        }
                    }
                })
            },
            deep: true
        }
    },
    computed: {
        sectionsKeys() {
            return Object.keys(this.sections)
        }
    },
    components: {
        Section,
        Label,
        Input,
        Select,
        Button
    }
}
</script>

<style lang="scss" scoped>
    @import "@/scss/variables.scss";

    .form {
        max-width: 600px;
        margin: 0 auto;
        border: $border;
        border-radius: 8px;
        box-sizing: border-box;
    }

    .form-header {
        padding: 20px;
        background: $blue5;
        border-bottom: $border;
    }

    .grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-gap: 10px 15px;
        margin: 20px 0;

        &.client {
            grid-template-columns: repeat(2, 1fr);
        }
    }

    .type-of-document,
    .issued-by {
        grid-column-start: 1;
        grid-column-end: 4;
    }

    .error {

        label {
            color: $error;
        }
        
        input {
            border: 1px solid $error;

            &:hover,
            &:focus {
                box-shadow: 0 0 5px $error;
            }
        }
    }

    .form-footer {
        padding: 20px;
        display: flex;
        justify-content: space-between;
        align-items: center;

        .success {
            padding: 10px 20px;
            color: green;
            border: 1px solid green;
            border-radius: 4px;
            box-shadow: 0 0 5px green;
            text-align: center;
        }
    }

    .notifications {
        cursor: pointer;

        input[type="checkbox"] {
            display: none;
        }

        input[type="checkbox"] + label {
            position: relative;
            padding-left: 30px;
        }

        input[type="checkbox"] + label::before {
            content: '';
            display: block;
            width: 20px;
            height: 20px;
            position: absolute;
            left: 0;
            top: calc(50% - 11px);
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
            top: 0;
            left: 7px;
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

    @media screen and (max-width: 600px) {
        .form {
            max-width: 90%;
        }

        .grid,
        .grid.client {
            grid-template-columns: none;
        }

        .type-of-document,
        .issued-by {
            grid-column-start: auto;
            grid-column-end: auto;
        }

        .form-footer {

            .success {
                max-width: 30%;
            }
        }
    }

    @media screen and (max-width: 500px) {        
        .form-footer {
            flex-direction: column;

            .success {
                max-width: 100%;
            }

            button {
                margin-top: 10px;
            }
        }
    }
</style>