<template>
    <table>
        <tr v-for="(row, i) in localMatrix" :key="i">
            <td v-for="(val, j) in row" :key="j">
                <input type="text" v-model.number="localMatrix[i][j]" @input="handleInput(i, j)" />
            </td>
        </tr>
    </table>
</template>

<script>
export default {
    props: ['matrix'],
    data() {
        return {
            localMatrix: JSON.parse(JSON.stringify(this.matrix))
        };
    },
    watch: {
        matrix(newVal) {
            this.localMatrix = JSON.parse(JSON.stringify(newVal));
        }
    },
    methods: {
        handleInput(i, j) {
            if (this.localMatrix[i][j] === null || this.localMatrix[i][j] === '') {
                this.localMatrix[i].splice(j, 1);
                if (this.localMatrix[i].length === 0) {
                    this.localMatrix.splice(i, 1);
                }
            } else {
                if (j === this.localMatrix[i].length - 1) this.localMatrix[i].push(null);
                if (i === this.localMatrix.length - 1) this.localMatrix.push([null]);
            }
            this.$emit('update-matrix', this.localMatrix);
        }
    }
};
</script>
