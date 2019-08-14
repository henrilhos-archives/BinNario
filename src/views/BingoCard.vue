<template>
  <VRow align="center">
    <VCol class="text-center display-1" cols="12">
      <span v-if="verifyCard">🎉</span>
      Cartela
      <span v-if="verifyCard">🎉</span>
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
              >{{ num.value }}</VCard
            >
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
  computed: {
    verifyCard() {
      const selecteds = this.bingoCard.map(item => item.selected);
      const lines = [
        selecteds.splice(0, 5),
        selecteds.splice(0, 5),
        selecteds.splice(0, 5),
        selecteds.splice(0, 5),
        selecteds.splice(0, 5)
      ];

      if (this.verifyLines(lines)) return true;
      if (this.verifyColumns(lines)) return true;
      if (this.verifyDiagonal(lines)) return true;

      return false;
    }
  },
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
      list.splice(12, 0, { value: 'BINGO', selected: true });

      return list;
    },
    verifyLines(lines) {
      let hasWon = false;

      lines.forEach(line => {
        if (line.indexOf(false) === -1) hasWon = true;
      });

      return hasWon;
    },
    verifyColumns(lines) {
      const columns = [];
      for (let i = 0; i < 5; i += 1) {
        const column = [];
        lines.forEach(line => {
          column.push(line[i]);
        });

        columns.push(column);
      }

      if (this.verifyLines(columns)) return true;
      return false;
    },
    verifyDiagonal(lines) {
      const diagonals = [[], []];

      let inverse = 4;
      for (let i = 0; i < 5; i += 1) {
        diagonals[0].push(lines[i][i]);
        diagonals[1].push(lines[i][inverse]);

        inverse -= 1;
      }

      if (this.verifyLines(diagonals)) return true;
      return false;
    }
  }
};
</script>
