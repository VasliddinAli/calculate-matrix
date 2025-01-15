<template>
    <div>
        <table>
            <tr v-for="(row, i) in localMatrix" :key="i">
                <td v-for="(val, j) in row" :key="j">
                    <input type="text" v-model.number="localMatrix[i][j]" @input="handleInput(i, j)" />
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
export default {
    props: {
        matrix: {
            type: Array,
            required: true
        }
    },
    data() {
        return {
            localMatrix: this.copyMatrix(this.matrix) // Propsning nusxasi
        };
    },
    watch: {
        matrix: {
            handler(newMatrix) {
                this.localMatrix = this.copyMatrix(newMatrix); // Props o'zgarishini kuzatish
            },
            deep: true
        }
    },
    methods: {
        copyMatrix(matrix) {
            return matrix.map(row => [...row]); // Matritsa nusxasini qaytarish
        },
        handleInput(i, j) {
            // Qiymat null yoki bo'sh bo'lsa, unga 0 ni tayyorlash
            if (this.localMatrix[i][j] === null || this.localMatrix[i][j] === '') {
                this.localMatrix[i][j] = 0;
            }

            // Qator oxirida yangi ustun qo'shish
            if (j === this.localMatrix[i].length - 1 && this.localMatrix[i][j] !== null) {
                this.localMatrix[i].push(null);
            }

            // Oxirgi qatorda yangi qator qo'shish
            if (i === this.localMatrix.length - 1 && this.localMatrix[i][j] !== null) {
                const newRow = Array(this.localMatrix[i].length).fill(null);
                this.localMatrix.push(newRow);
            }

            // Yangilangan matritsani ota komponentga yuborish
            this.updateMatrix();
        },
        updateMatrix() {
            this.$emit('update-matrix', this.copyMatrix(this.localMatrix)); // Props o'zgarishini zarar yetkazmaslik uchun nusxa olish
        }
    }
};
</script>