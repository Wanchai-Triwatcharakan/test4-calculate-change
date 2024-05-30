<template>
  <div>
    <div class="container">
      <h1>โปรแกรมคำนวณเงินทอน</h1>
      <div class="container-content">
        <label for="price">ราคาสินค้า:</label>
        <input type="number" v-model="price">
      </div>
      <div class="container-content">
        <label for="money">รับเงินมา:</label>
        <input type="number" v-model="receiveMoney">
      </div>
      <div class="container-button">
        <button @click="calculateChange" class="submit">คำนวณ</button>
        <button @click="reset" class="reset">ล้างค่า</button>
      </div>
    </div>

    <div v-if="change !== null">
      <div class="content">
        <div v-if="Object.keys(change).length > 0">
          <h2>จะต้องทอนเงิน:</h2>
          <div v-for="(item, index) in Object.entries(change).reverse()" :key="index">
            {{ item[0] }} บาท = {{ item[1] }} {{ item[0] >= 50 ? 'ใบ' : 'เหรียญ' }}
          </div>
        </div>
        <div v-else>
          <h2>รับเงินมาพอดีกับราคาสินค้า</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      price: null,
      receiveMoney: null,
      valueOfMoney: [500, 100, 50, 10, 5, 1],
      change: null
    };
  },
  methods: {
    calculateChange() {
      if (this.price === null || this.receiveMoney === null || this.price <= 0 || this.receiveMoney <= 0) {
        alert("กรุณาป้อนราคาสินค้าหรือจำนวนเงินที่รับมา โดยที่มีค่ามากกว่า 0");
        return;
      } else {
        const changeAmount = this.receiveMoney - this.price;
        if (changeAmount < 0) {
          alert("จำนวนเงินที่รับมาน้อยกว่าราคาสินค้า กรุณาเพิ่มเงิน");
          return;
        }
        let remainingMoney = changeAmount;
        const change = {};
        this.valueOfMoney.forEach(oneValueOfMoney => {
          const count = Math.floor(remainingMoney / oneValueOfMoney);
          if (count > 0) {
            change[oneValueOfMoney] = count;
            remainingMoney = remainingMoney - (count * oneValueOfMoney);
          }
        });
        this.change = change;
      }
    },
    reset() {
      this.price = null,
      this.receiveMoney = null,
      this.change = null
    }
  }
};
</script>

<style>
*{
  font-family: Kanit;
}

.container {
  display: block;
  text-align: center;
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.25);
  padding: 2rem;
  background: #fff;
}

label {
  font-size: 18px;
  font-weight: bold;
}

.container-content{
  margin: 1rem 0;
  text-align: start;
}

input {
  display: block;
  width: 100%;
  margin-top: 0.5rem;
}

button {
  display: inline-block;
  font: inherit;
  color: white;
  cursor: pointer;
  padding: 0.75rem 2rem;
  margin: 1rem 0.5rem;
  border-radius: 15px;
}

.reset {
  background: red;
}

.reset:hover {
  background: orangered;
}

.submit {
  background: green;
}

.submit:hover {
  background: rgb(0, 150, 0);
}

.content {
  display: block;
  text-align: center;
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.25);
  padding: 2rem;
  background: #fff;
}

p {
  font-size: 18px;
}
</style>