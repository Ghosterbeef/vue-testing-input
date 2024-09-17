<template>
    <div class="field">
        <label v-if="label" class="field__label" :for="id">{{ label }}</label>
        <div class="field__wrapper" :class="{'field__wrapper--error': !isValid.isValid}">
            <div v-if="$slots.prepend" class="field__prepend">
                <slot name="prepend"/>
            </div>
            <input
                :id="id" class="field__input"
                :value="modelValue"
                :type="type"
                :placeholder="placeholder"
                :disabled="disabled"
                @input="onInput"
            >
            <div v-if="clearable && modelValue && !disabled || $slots.append" class="field__append">
                <slot name="append"/>
                <button v-if="clearable && modelValue && !disabled" class="field__clear-btn" @click="clear">
                    &times;
                </button>
            </div>
        </div>
        <div class="field__messages">
            <span class="field__hint" v-if="hint && isValid.isValid">{{ hint }}</span>
            <span class="field__error" v-if="!isValid.isValid && isValid.message">{{ isValid.message }}</span>
        </div>
    </div>
</template>

<script setup lang="ts">
import {computed} from "vue";

interface Props {
    id?: string | number;
    label?: string;
    placeholder?: string;
    type?: string;
    disabled?: boolean;
    clearable?: boolean;
    modelValue: string;
    hint?: string;
    required?: boolean;
    minValue?: number;
    maxValue?: number;
}

interface Emits {
    (e: 'update:modelValue', value: string): void;
}

const props = withDefaults(defineProps<Props>(), {
    id: Math.random,
});

const emit = defineEmits<Emits>();

const validators = computed(() => {
    const result: ((value: string) => { isValid: boolean, message: string })[] = [];

    if (props.required) {
        result.push((value: string) => value ? {isValid: true, message: ''} : {
            isValid: false,
            message: 'Обязательное поле'
        })
    }

    if (typeof props.minValue === 'number') {
        result.push((value: string) => value.length < props.minValue ? {isValid: true, message: ''} : {
            isValid: false,
            message: `Минимальная длина составляет ${props.minValue}`
        })
    }

    if (typeof props.maxValue === 'number') {
        result.push((value: string) => value.length > props.maxValue ? {isValid: true, message: ''} : {
            isValid: false,
            message: `Максимальная длина составляет ${props.maxValue}`
        })
    }

    return result
})

const isValid = computed(() => {
    const result = {isValid: true, message: ''}

    for (const validator of validators.value) {
        const validationResult = validator(props.modelValue);
        if (!validationResult.isValid) {
            result.isValid = false;
            result.message = validationResult.message;
            break;
        }
    }

    return result;
})

function clear() {
    if (props.clearable) {
        emit('update:modelValue', '');
    }
}

function onInput(event: Event) {
    if (event.target instanceof HTMLInputElement) {
        emit('update:modelValue', event.target.value);
    }
}

</script>

<style scoped>
.field__wrapper {
    display: flex;
}
</style>
