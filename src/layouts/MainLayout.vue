<template>
  <div :style="{
    display: 'flex',
    flexDirection: 'column',
    justifyContent: 'start',
    alignItems: 'center',
    width: '100%',
  }">
  <h3>Калькулятор амортизации</h3>
  <q-select class="choose-method" filled v-model="chosen" :options="methods" label="Filled" />
  <LinearMethod class="linear-method" v-if="chosen === 'Линейный метод'"/>
  </div>
</template>

<script setup lang="ts">
import LinearMethod from 'pages/LinearMethod.vue';
import { ref } from 'vue';

const methods = ['Линейный метод', 'Метод уменьшаемого остатка', 'Метод начисления амортизации в зависимости от объема выпуска продукции']
const chosen = ref('')
</script>

<style scoped>
.choose-method {
  width: 500px;
}

.linear-method {
  width: 100%
}
</style>
