<template>
    <div class="controls">
        <label for="disabled">disabled<input id="disabled" type="checkbox" v-model="disabled"></label>
        <label for="clearable">clearable<input id="clearable" type="checkbox" v-model="clearable"></label>
        <label for="required">required<input id="required" type="checkbox" v-model="required"></label>
        <label for="minValue">minValue<input id="minValue" type="tel" v-model.number="minValue"></label>
        <label for="maxValue">maxValue<input id="maxValue" type="tel" v-model.number="maxValue"></label>
    </div>
    <VInput
        label="Тестовое поле ввода"
        placeholder="Введите свой текст"
        v-model="inputValue"
        :disabled="disabled"
        :clearable="clearable"
        :required="required"
        :min-value="minValue"
        :max-value="maxValue"
    />
    {{ inputValue }}
</template>

<script setup lang="ts">
import VInput from "@/components/VInput.vue";
import {ref} from "vue";

const disabled = ref(false);
const clearable = ref(false);
const required = ref(false);
const minValue = ref(0);
const maxValue = ref(0)

const inputValue = ref('');
</script>

<style scoped>
.controls {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

label {
    display: flex;
    gap: 8px;
}
</style>
