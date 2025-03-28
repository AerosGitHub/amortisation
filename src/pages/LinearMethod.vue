<script setup lang="ts">
import type { Ref} from 'vue';
import { ref } from 'vue';
import type { QTableProps } from 'quasar';

const price = ref('');
const time = ref('');
const exitPrice = ref('');

interface OneMonth {
  name: number,
  monthly_dead: number,
  sum_dead: number,
  remain_price: number,
}

const columns: QTableProps['columns'] = [
  {
    name: 'name',
    required: true,
    label: 'Период',
    align: 'left',
    field: 'name'
  },
  { name: 'monthly_dead', label: 'Месячная сумма износа', field: 'monthly_dead' },
  { name: 'sum_dead', label: 'Накопленный износ', field: 'sum_dead' },
  { name: 'remain_price', label: 'Остаточная стоимость', field: 'remain_price' }
];

const rows: Ref<OneMonth[], OneMonth[]> = ref([]);


function calculateAmortisation() {
  const amortPrice = Number(price.value) - Number(exitPrice.value);
  const monthAmort = Number((amortPrice / Number(time.value)).toFixed(2));
  let sum_dead_counting = 0;
  for (let i = 1; i <= Number(time.value); i++) {
    sum_dead_counting += monthAmort;
    sum_dead_counting = Number(sum_dead_counting.toFixed(2))
    rows.value.push({
      name: i,
      monthly_dead: monthAmort,
      sum_dead: sum_dead_counting,
      remain_price:Number((Number(price.value) - sum_dead_counting).toFixed(2))
    });
  }
}
</script>

<template>
  <div :style="{
    width: '100%',
  }">
    <div class="input-container">
      <q-input class="input" id="price" standout v-model="price" label="Цена ОС" />
      <q-input class="input" id="time" standout v-model="time" label="Срок полезного использования (в месяцах)" />
      <q-input class="input" id="exit-price" standout v-model="exitPrice" label="Ликвидационная стоимость" />
    </div>
    <q-btn color="secondary" label="Рассчитать" @click="calculateAmortisation" />
    <q-table
      title="Рассчет амортизации"
      :rows="rows"
      :columns="columns"
      row-key="name"
    />
  </div>
</template>

<style scoped>
.input-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

.input {
  width: 300px;
  margin: 10px;
}
</style>
