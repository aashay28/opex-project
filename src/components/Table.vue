<template>
  <div class="p-4">
    <div class="overflow-auto rounded-lg">
      <table class="w-full border-collapse text-sm">
        <thead>
          <tr class="text-left bg-gray-200">
            <th
              v-for="(col, index) in headers"
              :key="index"
              class="p-2 text-left border border-gray-300"
              :class="index === 1 || index === 0 ? 'min-w-50' : 'min-w-30'"
            >
              {{ col }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(row, rowIndex) in computedTableData"
            :key="rowIndex"
            class="hover:bg-gray-100"
          >
            <td
              v-for="(cell, colIndex) in row"
              :key="colIndex"
              class="p-2 text-left border border-gray-200 text-xs"
              :class="[
                rowIndex <= 1 ? 'bg-gray-600 text-white' : '',
                lockedCells.has(`${rowIndex},${colIndex}`)
                  ? 'bg-gray-300 cursor-not-allowed'
                  : 'bg-white cursor-text',
              ]"
            >
              <input
                v-if="!lockedCells.has(`${rowIndex},${colIndex}`)"
                v-model="computedTableData[rowIndex][colIndex]"
                class="w-full bg-transparent text-left focus:outline-none min-w-30"
                @input="
                  updateCell(
                    rowIndex,
                    colIndex,
                    computedTableData[rowIndex][colIndex]
                  )
                "
              />
              <span v-else>{{ cell }}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const headers = ref([
  "Summary Totals",
  "GL Group",
  "2024-Jan",
  "2024-Feb",
  "2024-Mar",
  "2024-Apr",
  "2024-May",
  "2024-Jun",
  "2024-Jul",
  "2024-Aug",
  "2024-Sep",
  "2024-Oct",
  "2024-Nov",
  "2024-Dec",
  "2025-Jan",
  "2025-Feb",
  "2025-Mar",
  "2025-Apr",
  "2025-May",
  "2025-Jun",
  "2025-Jul",
  "2025-Aug",
  "2025-Sep",
  "2025-Oct",
  "2025-Nov",
  "2025-Dec",
  "2024 Jan - 2025 Dec Total",
  "Avg 2024 Jan - 2024 Dec",
  "Avg 2025 Jan - 2025 Dec",
]);
const baseTableData = ref([
  [
    "Grand Total",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
  ],
  [
    "Fixed Operating Expenses ($)",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
  ],
  [
    "",
    "Field Controllable",
    "$ 25,666",
    "$ -13,362",
    "$ 26,542",
    "$ 15,375",
    "$ 17,472",
    "$ 13,814",
    "$ 14,125",
    "$ 14,355",
    "$ 14,100",
    "$ 13,013",
    "$ 68,346",
    "$ 68,523",
    "$ 26,757",
    "$ 19,829",
    "$ 27,352",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "$ 351,907",
    "23164.083",
    "6161.50",
  ],
  [
    "",
    "Surface Repairs & Maintenance",
    "$ 251,909",
    "$ 225,418",
    "$ 249,218",
    "$ 255,952",
    "$ 279,273",
    "$ 271,388",
    "$ 282,663",
    "$ 274,800",
    "$ 266,848",
    "$ 291,883",
    "$ 281,550",
    "$ 271,578",
    "$ 319,705",
    "$ 331,497",
    "$ 347,491",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "4,201,173",
    "266873.333",
    "83224.42",
  ],
  [
    "",
    "Equipment Rentals",
    "$ 32,184",
    "$ 34,834",
    "$ 44,601",
    "$ 32,184",
    "$ 50,675",
    "$ 85,667",
    "$ 79,658",
    "$ 102,431",
    "$ 80,381",
    "$ 32,184",
    "$ 32,179",
    "$ 32,326",
    "$ 111,384",
    "$ 101,111",
    "$ 107,020",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "$ 958,819",
    "53275.333",
    "26626.25",
  ],
  [
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
  ],
  [
    "Total Variable Opex ($)",
    "",
    "$ 127,610",
    "$ 170,431",
    "$ 293,884",
    "$ 355,504",
    "$ 259,877",
    "$ 146,668",
    "$ 95,761",
    "$ 112,185",
    "$ 73,103",
    "$ 104,439",
    "$ 112,129",
    "$ 139,359",
    "$ 159,942",
    "$ 199,721",
    "$ 220,003",
    "$ -",
    "$ -",
    "$ -",
    "$ -",
    "$ -",
    "$ -",
    "$ -",
    "$ -",
    "$ -",
    "$ -",
    "$ 165,913",
    "$ 579,666",
  ],
  [
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
  ],
  [
    "Variable Opex (Rates) Total ($/BOE)",
    "",
    "0.37",
    "0.48",
    "0.72",
    "0.93",
    "0.65",
    "0.38",
    "0.25",
    "0.29",
    "0.19",
    "0.27",
    "0.29",
    "0.36",
    "0.38",
    "0.57",
    "0.57",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "6.70",
    "0.43",
    "1.52",
  ],
  [
    "",
    "Emulsion Handling",
    "0.04",
    "0.03",
    "0.03",
    "0.01",
    "0.02",
    "0.02",
    "0.02",
    "0.02",
    "0.00",
    "0.01",
    "0.00",
    "0.00",
    "0.03",
    "0.00",
    "0.00",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0.23",
    "0.02",
    "0.03",
  ],
  [
    "",
    "Gas Handling Third Party",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0.00",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0.00",
    "0.00",
    "0.00",
  ],
  [
    "",
    "Produced Water Handling/Disposal",
    "0.33",
    "0.45",
    "0.69",
    "0.92",
    "0.63",
    "0.36",
    "0.23",
    "0.27",
    "0.19",
    "0.26",
    "0.29",
    "0.36",
    "0.35",
    "0.57",
    "0.57",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "0",
    "6.47",
    "0.42",
    "1.49",
  ],
  [
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
  ],
  [
    "Total Volume (boe)",
    "",
    "351,813",
    "351,647",
    "409,118",
    "382,754",
    "400,304",
    "387,298",
    "387,345",
    "394,160",
    "374,266",
    "394,692",
    "378,418",
    "382,798",
    "421,853",
    "350,388",
    "385,969",
    "366,844",
    "370,988",
    "350,966",
    "352,394",
    "270,458",
    "326,207",
    "329,313",
    "311,116",
    "313,847",
    "8,744,956",
    "382,884",
    "4,150,343",
  ],
  [
    "Total Opex ($/boe)",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
    "",
  ],
]);

const computedTableData = computed(() => {
  let newTableData = JSON.parse(JSON.stringify(baseTableData.value));

  let grandTotalRow = Array(29).fill(0);

  for (let i = 2; i <= 25; i++) {
    let fixedOpex =
      parseCurrency(newTableData[2][i]) +
      parseCurrency(newTableData[3][i]) +
      parseCurrency(newTableData[4][i]);

    newTableData[1][i] = formatCurrency(fixedOpex);

    let variableOpex =
      parseCurrency(newTableData[9][i]) +
      parseCurrency(newTableData[10][i]) +
      parseCurrency(newTableData[11][i]);

    newTableData[8][i] = formatCurrency(variableOpex);

    let grandTotal = fixedOpex + parseCurrency(newTableData[6][i]);
    newTableData[0][i] = formatCurrency(grandTotal);

    grandTotalRow[i] = grandTotal;

    let totalVolume = parseCurrency(newTableData[13][i]);
    newTableData[14][i] = `$ ${
      totalVolume !== 0 ? (grandTotal / totalVolume).toFixed(2) : "0.00"
    }`;
  }

  [2, 3, 4, 6, 8, 9, 10, 11, 13, 14].forEach((row) => {
    let total = 0;
    let sum2024 = 0;
    let sum2025 = 0;

    for (let i = 2; i <= 25; i++) {
      let value = parseCurrency(newTableData[row][i]);

      total += value;
      if (i <= 13) sum2024 += value;
      else sum2025 += value;
    }

    newTableData[row][26] = formatCurrency(total);
    newTableData[row][27] = formatCurrency(sum2024 / 12);
    newTableData[row][28] = formatCurrency(sum2025 / 12);
  });

  let grandTotal = 0;
  let grandTotal2024 = 0;
  let grandTotal2025 = 0;

  for (let i = 2; i <= 25; i++) {
    grandTotal += grandTotalRow[i];
    if (i <= 13) grandTotal2024 += grandTotalRow[i];
    else grandTotal2025 += grandTotalRow[i];
  }

  newTableData[0][26] = formatCurrency(grandTotal);
  newTableData[0][27] = formatCurrency(grandTotal2024 / 12);
  newTableData[0][28] = formatCurrency(grandTotal2025 / 12);

  return newTableData;
});

const lockedCells = ref(
  new Set([
    ...Array.from(
      { length: baseTableData.value.length },
      (_, row) => `${row},0`
    ),
    ...Array.from(
      { length: baseTableData.value.length },
      (_, row) => `${row},1`
    ),
    ...Array.from(
      { length: baseTableData.value.length },
      (_, row) => `${row},26`
    ),
    ...Array.from(
      { length: baseTableData.value.length },
      (_, row) => `${row},27`
    ),
    ...Array.from(
      { length: baseTableData.value.length },
      (_, row) => `${row},28`
    ),
    ...Array.from({ length: 29 }, (_, col) => `0,${col}`),
    ...Array.from({ length: 29 }, (_, col) => `1,${col}`),
    ...Array.from({ length: 29 }, (_, col) => `6,${col}`),
    ...Array.from({ length: 29 }, (_, col) => `8,${col}`),
    ...Array.from({ length: 29 }, (_, col) => `13,${col}`),
    ...Array.from({ length: 29 }, (_, col) => `14,${col}`),
  ])
);

const updateCell = (rowIndex, colIndex, value) => {
  if (!lockedCells.value.has(`${rowIndex},${colIndex}`)) {
    baseTableData.value[rowIndex][colIndex] = value;
  }
};

const parseCurrency = (str) => Number(str.replace(/[$,]/g, "")) || 0;
const formatCurrency = (num) => `$ ${num.toLocaleString()}`;
</script>
