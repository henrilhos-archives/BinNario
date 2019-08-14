<template>
  <VRow align="center">
    <VCol class="text-center display-1" cols="12">
      Cartela
    </VCol>
    <VCol cols="12">
      <VCard class="py-5 text-center">
        <VRow>
          <VCol
            v-for="(num, n) in bingoCard"
            :key="n"
            cols="2"
            :offset="borders.indexOf(n) != -1 ? '1' : '0'"
            style="padding: 1px !important;"
          >
            <VCard
              flat
              outlined
              tile
              class="pa-2 text-center"
              :color="num.selected ? 'grey lighten-1' : ''"
              @click="num.selected = !num.selected"
            >
              {{ num.value }}
            </VCard>
          </VCol>
        </VRow>
      </VCard>
    </VCol>
  </VRow>
</template>

<script>
import faker from 'faker';
import decimals from '@/assets/decimals.json';

export default {
  data: () => ({
    bingoCard: undefined,
    borders: [0, 5, 10, 15, 20]
  }),
  created() {
    const numList = this.shuffle();

    this.bingoCard = this.getNumbers(numList);
  },
  methods: {
    shuffle() {
      return faker.helpers.shuffle(decimals);
    },
    getNumbers(numList) {
      let list = [];
      for (let i = 1; i < 25; i += 1) {
        list.push({ value: numList.shift(), selected: false });
      }

      list = list.sort();
      list.splice(12, 0, { value: 'BINGO', selected: false });

      return list;
    }
  }
};
</script>
