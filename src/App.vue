<template>
  <div>
    <h1>Matrix Calculator</h1>
    <div>
      <button :class="{ active: operation === 'add' }" @click="setOperation('add')">Qo'shish</button>
      <button :class="{ active: operation === 'subtract' }" @click="setOperation('subtract')">Ayirish</button>
      <button :class="{ active: operation === 'scalar' }" @click="setOperation('scalar')">Songa Ko'paytirish</button>
      <button :class="{ active: operation === 'multiply' }" @click="setOperation('multiply')">Ko'paytirish</button>
      <button :class="{ active: operation === 'divide' }" @click="setOperation('divide')">Bo'lish</button>
    </div>
    <MatrixInput :matrix="matrix1" @update-matrix="updateMatrix1" />
    <MatrixInput :matrix="matrix2" @update-matrix="updateMatrix2" />
    <button @click="calculate">Hisoblash</button>
    <h2>Natija</h2>
    <p v-if="error" style="color: red;">Notog'ri</p>
    <table v-else>
      <tr v-for="(row, i) in result" :key="i">
        <td v-for="(val, j) in row" :key="j">{{ formatValue(val) }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import MatrixInput from './components/MatrixInput.vue';

export default {
  components: { MatrixInput },
  data() {
    return {
      operation: 'add',
      matrix1: [[null]],
      matrix2: [[null]],
      result: [],
      error: false
    };
  },
  methods: {
    setOperation(op) {
      this.operation = op;
    },
    updateMatrix1(newMatrix) {
      this.matrix1 = JSON.parse(JSON.stringify(newMatrix));
    },
    updateMatrix2(newMatrix) {
      this.matrix2 = JSON.parse(JSON.stringify(newMatrix));
    },
    calculate() {
      try {
        this.error = false;
        const m1 = this.matrix1;
        const m2 = this.matrix2;

        const fillMatrix = (matrix) => matrix.map(row => row.map(val => val === null ? 0 : val));

        const filledM1 = fillMatrix(m1);
        const filledM2 = fillMatrix(m2);

        if (this.operation === 'add') {
          if (filledM1.length !== filledM2.length || filledM1[0].length !== filledM2[0].length) throw 'error';
          this.result = filledM1.map((row, i) => row.map((val, j) => val + filledM2[i][j]));
        }
        else if (this.operation === 'subtract') {
          if (filledM1.length !== filledM2.length || filledM1[0].length !== filledM2[0].length) throw 'error';
          this.result = filledM1.map((row, i) => row.map((val, j) => val - filledM2[i][j]));
        }
        else if (this.operation === 'scalar') {
          const scalar = parseFloat(prompt('Songa ko\'paytirish qiymatini kiriting:'));
          if (isNaN(scalar)) throw 'error';
          this.result = filledM1.map(row => row.map(val => val * scalar));
        }
        else if (this.operation === 'multiply') {
          if (filledM1[0].length !== filledM2.length) throw 'error';
          this.result = filledM1.map(row => filledM2[0].map((_, j) => row.reduce((sum, val, k) => sum + val * filledM2[k][j], 0)));
        }
        else if (this.operation === 'divide') {
          if (filledM1.length !== filledM2.length || filledM1[0].length !== filledM2[0].length) throw 'error';
          this.result = filledM1.map((row, i) => row.map((val, j) => {
            const divisor = filledM2[i][j] === 0 ? 1 : filledM2[i][j];
            return val / divisor;
          }));
        } else {
          throw 'error';
        }
      } catch (e) {
        this.error = true;
      }
    },
    formatValue(value) {
      if (value === 0) return '';
      return value;
    }
  }
};
</script>
