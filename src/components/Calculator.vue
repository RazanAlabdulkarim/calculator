<template>
  <div class="calculator">
    <!-- Display the calculator title -->
    <h1>{{ msg }}</h1>

    <div class="calculator-body">
      <table class="calculator-table">
        <tbody>
          <!-- Display the output row -->
          <tr class="output">
            <td colspan="4">
              {{ displayOutput || 0 }}
            </td>
          </tr>

          <!-- Rows for buttons -->
          <!-- Row 1 -->
          <tr>
            <td class="lastColumn" @click="clearField">C</td>
            <td class="lastColumn" @click="setNegativeOrPositive">+/-</td>
            <td class="lastColumn" @click="calculatePercentage">%</td>
            <td class="lastColumn" @click="processOutput('divide')">
                <i class="fas fa-solid fa-divide"></i>
            </td>
          </tr>

          <!-- Row 2 -->
          <tr>
            <td @click="getNumber('7')">7</td>
            <td @click="getNumber('8')">8</td>
            <td @click="getNumber('9')">9</td>
            <td class="lastColumn" @click="processOutput('multiply')">x</td>
          </tr>

          <!-- Row 3 -->
          <tr>
            <td @click="getNumber('4')">4</td>
            <td @click="getNumber('5')">5</td>
            <td @click="getNumber('6')">6</td>
            <td class="lastColumn" @click="processOutput('subtract')">-</td>
          </tr>

          <!-- Row 4 -->
          <tr>
            <td @click="getNumber('1')">1</td>
            <td @click="getNumber('2')">2</td>
            <td @click="getNumber('3')">3</td>
            <td class="lastColumn" @click="processOutput('add')">+</td>
          </tr>

          <!-- Row 5 -->
          <tr>
            <td colspan="2" @click="getNumber('0')">0</td>
            <td @click="getDot">.</td>
            <td class="lastColumn" @click="updateOutput">=</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<!------------------------------------------------------------------------->
<script>
export default {
  name: 'CalculatorApp',
  props: {
    msg: String
  },
  data() {
    return {
      // Data for managing calculator state
      output: '',
      previousValue: null,
      operation: null,
      operationFired: false,
      maxDigits: 12
    };
  },
  computed: {
    displayOutput() {
      if (this.output) {
        // Convert the output to a number
        const numberValue = parseFloat(this.output);

        // Format the number with commas 
        const formattedOutput = numberValue.toLocaleString('en');

      return formattedOutput;
      }

    return this.output; 
    }
  },

  methods: {
    // Method to clear the output field
    clearField() {
      this.output = '';
    },
    // Method to toggle between positive and negative values
    setNegativeOrPositive() {
      this.output = this.output[0] === '-' ? this.output.slice(1) : `-${this.output}`;
    },
    // Method to calculate the percentage value of the output
    calculatePercentage() {
      this.output = parseFloat(this.output) / 100;
    },
    // Method to handle number button clicks and limit the number of digits
    getNumber(number) {
      if (this.operationFired) {
        this.output = '';
        this.operationFired = false;
      }
      if (this.output.length < this.maxDigits) {
        this.output = `${this.output}${number}`;
      }
    },
    // Method to handle decimal point (dot) button click and limit its usage
    getDot() {
      if (this.output.indexOf('.') === -1 && this.output.length < this.maxDigits) {
        this.output = this.output + '.';
      }
    },
    // Method to set the operation to be performed and store the previous value
    processOutput(operationString) {
      if (operationString === 'add') {
        this.operation = (a, b) => parseFloat(a) + parseFloat(b);
      } else if (operationString === 'subtract') {
        this.operation = (a, b) => parseFloat(a) - parseFloat(b);
      } else if (operationString === 'multiply') {
        this.operation = (a, b) => parseFloat(a) * parseFloat(b);
      } else if (operationString === 'divide') {
        this.operation = (a, b) => parseFloat(a) / parseFloat(b);
      }

      this.previousValue = this.output;
      this.operationFired = true;
    },
    // Method to perform the operation and update the output
    updateOutput() {
      if (this.operation) {
        this.output = `${this.operation(this.previousValue, this.output)}`;
      }
      this.previousValue = null;
      this.operation = null;
    }
  }
}
</script>
<!------------------------------------------------------------------------->
<style scoped>
/* Styles for the calculator */
.calculator {
  background-color: #f2f5f8;
  border-radius: 8px;
  padding: 20px;
  max-width: 300px;
  margin: 0 auto;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.calculator-body {
  margin-top: 15px;
}

.calculator-table {
  width: 100%;
  border-radius: 6px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.calculator-table tr {
  height: 50px;
}

.calculator-table td {
  text-align: center;
  vertical-align: middle;
  font-size: 20px;
  font-weight: 600;
  color: #3498db;
  cursor: pointer;
  border: 1px solid #ccc;
}

.calculator-table .output td {
  color: #fff;
  background-color: #3498db;
}

.calculator-table td:hover {
  background-color: #ebf3f9;
}

.lastColumn{
  background-color: #8ac8f8;
}

h1 {
  color: #2980b9;
}
</style>
