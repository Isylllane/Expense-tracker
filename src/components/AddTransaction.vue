<template>
    <h3>Добавить транзакцию</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
        <label for="text">Наименование</label>
            <input 
                type="text" 
                id="text" 
                placeholder="Введите текст..." 
                v-model="text"
            />
        </div>
        <div class="form-control">
        <label for="amount"
            >Сумма
            </label>
        <input
            type="text"
            id="amount"
            placeholder="Введите сумму..."
            v-model.number="amount"
        />
        <label>
            <input type="radio" v-model="activeComponent" :value="CompA"> Доход
        </label>
        <label>
            <input type="radio" v-model="activeComponent" :value="CompB"> Расход
        </label>
        <Transition name="fade" mode="out-in">
            <component :is="activeComponent"></component>
        </Transition>
        </div>

        <button class="btn">Добавить</button>

    </form>
</template>
<script setup>
    import { ref } from 'vue';
    import { shallowRef } from 'vue';
    import CompA from './CompA.vue';
    import CompB from './CompB.vue';
    import { useToast } from 'vue-toastification';

    const activeComponent = shallowRef(CompA);
    const toast = useToast();
    const emit = defineEmits(['transactionsSubmitted']);

    const text = ref('');
    const amount = ref('');

    const onSubmit = () => {
        if(!text.value || !amount.value) {
            toast.error('Все поля должны быть заполнены');
        } else {
            const transactionData = {
                text: text.value,
                amount:parseFloat(amount.value)
            }
        emit('transactionsSubmitted', transactionData);
        }

        text.value = '';
        amount.value = '';
    }
</script>