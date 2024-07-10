<template>
    <div class="inc-exp-container">
        <div>
            <h4>Доход</h4>
            <p id="money-plus" class="money plus">+{{ income }} ₽</p>
        </div>
        <div>
            <h4>Расход</h4>
            <p class="money-minus money minus">{{ expenses }} ₽</p>
        </div>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';
const props = defineProps({
    income: {
        type: Number,
        required: true,
    },
    expenses: {
        type: Number,
        required: true,
    },
});
</script>