<template>
  <VRow>
    <VCol class="display-1" cols="12">
      BinNário
    </VCol>
    <VCol cols="12">
      <VCard class="pa-5 text-center">
        <div v-if="actualNumber" class="display-4 mb-6">
          {{ actualNumber }}
        </div>
        <VBtn
          large
          color="primary"
          :disabled="numbers.length == 0"
          @click="getNumber()"
          >Sortear</VBtn
        >
      </VCard>
    </VCol>
    <VCol cols="12">
      <VCard class="text-center pa-5">
        <VRow>
          <VCol
            v-for="num in sortedNumbers"
            :key="num"
            cols="3"
            class="display-1"
            style="padding: 5px !important;"
            >{{ num }}</VCol
          >
        </VRow>
      </VCard>
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

      if (this.actualNumber) this.sortedNumbers.unshift(this.actualNumber);
      this.actualNumber = num;
    }
  }
};
</script>
