<template>
  <VRow>
    <VCol cols="12" md="8">
      <VCard class="pa-5 text-center">
        <div v-if="actualNumber" class="display-4 mb-6">
          {{ actualNumber }}
        </div>
        <VBtn large color="primary" @click="getNumber()">Sortear</VBtn>
      </VCard>
    </VCol>
    <VCol cols="12" md="4">
      <VCard class="pa-5">{{ sortedNumbers }}</VCard>
    </VCol>
  </VRow>
</template>

<script>
import faker from 'faker';
import binaries from '@/assets/binaries.json';

export default {
  data: () => ({
    numbers: undefined,
    actualNumber: undefined,
    sortedNumbers: []
  }),
  created() {
    this.numbers = this.shuffle();
  },
  methods: {
    shuffle() {
      return faker.helpers.shuffle(binaries);
    },
    getNumber() {
      const num = this.numbers.shift();

      if (this.actualNumber) this.sortedNumbers.push(this.actualNumber);
      this.actualNumber = num;
    }
  }
};
</script>
