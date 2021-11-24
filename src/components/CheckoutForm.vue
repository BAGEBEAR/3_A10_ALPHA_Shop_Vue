<template>
  <div class="lower-form">
    <div class="form-container">
      <h1 class="form-title">寄送地址</h1>
      <form @submit.stop.prevent="handleSubmit" id="form" class="form-info">
        <div v-show="toggleStep === 0" class="form-part">
          <div class="form-1">
            <div class="form-div gender">
              <label>稱謂</label>
              <div class="select-wrapper">
                <select name="gender" id="form-gender" v-model="checkout.gender">
                  <option value="male">先生</option>
                  <option value="female">小姐</option>
                </select>
              </div>
            </div>
            <div class="form-div name">
              <label>姓名</label>
              <input
                v-model="checkout.name"
                type="text"
                placeholder="請輸入姓名"
              />
            </div>
            <div class="form-div phone">
              <label>電話</label>
              <input
                type="text"
                placeholder="請輸入行動電話"
                v-model="checkout.tel"
              />
            </div>
            <div class="form-div email">
              <label>Email</label>
              <input
                type="text"
                placeholder="請輸入電子郵件"
                v-model="checkout.email"
              />
            </div>
            <div class="form-div city">
              <label>縣市</label>
              <select 
                v-model="checkout.city" 
                name="city" 
                id="form-city"
              >
                <option v-for="city in cities" :key="city.id" :value="city.name">
                  {{ city.name }}
                </option>
              </select>
            </div>
            <div class="form-div address">
              <label>地址</label>
              <input
                v-model="checkout.address"
                type="text"
                placeholder="請輸入地址"
              />
            </div>
          </div>
        </div>
        <div v-show="toggleStep === 1" class="form-part">
          <div class="form-2">
            <div class="deliver">
              <label for="deliver-0" class="deliver-row">
                <div class="deliver-input">
                  <input
                    @change="handleDeliverChange"
                    v-model="checkout.deliver"
                    id="deliver-0" 
                    name="deliver" 
                    type="radio" 
                    value= 0 
                  />
                </div>
                <div class="deliver-info">
                  <div class="deliver-info-text">
                    <strong>標準運送</strong>
                    <p>約 3~7 的工作天</p>
                  </div>
                  <div class="deliver-info-fee"><p>免費</p></div>
                </div>
              </label>
              <label for="deliver-1" class="deliver-row">
                <div class="deliver-input">
                  <input 
                    @change="handleDeliverChange"
                    v-model="checkout.deliver"
                    id="deliver-1" 
                    name="deliver" 
                    type="radio" 
                    value= 1
                  />
                </div>
                <div class="deliver-info">
                  <div class="deliver-info-text">
                    <strong>DHL 貨運</strong>
                    <p>48小時內送達</p>
                  </div>
                  <div class="deliver-info-fee"><p>$500</p></div>
                </div>
              </label>
            </div>
          </div>
        </div>
        <div v-show="toggleStep === 2" class="form-part">
          <div class="form-3">
            <div class="form-div pay-name">
              <label>持卡人姓名</label>
              <input 
                v-model="checkout.payment.name"
                type="text" 
                placeholder="請輸入姓名" 
              />
            </div>
            <div class="form-div pay-card">
              <label>卡號</label>
              <input 
                v-model="checkout.payment.cardNumber"
                type="text" 
                placeholder="1111 2222 3333 4444" />
            </div>
            <div class="form-div pay-card-exp">
              <label>有效期限</label>
              <input 
                v-model="checkout.payment.expDate"
                type="text" 
                placeholder="MM/YY" 
              />
            </div>
            <div class="form-div pay-card-cvc">
              <label>CVC/CCV</label>
              <input
                v-model="checkout.payment.cvc"
                type="text" 
                placeholder="123" 
              />
            </div>
          </div>
        </div>
      </form>
    </div>
    <div 
    class="container-btn py-6"
    v-bind:class="{ 
      'justify-content-end': toggleStep === 0, 
      'justify-content-between': toggleStep > 0}"  
  >
    <div
      v-show="toggleStep > 0"
      @click="togglePrevStep"
      class="btn btn-prev"
    >
      ← 上一步
    </div>
    <div
      @click="toggleNextStep"
      class="btn btn-next first-step"
    >
      {{toggleStep !== 2 ? "下一步 →" : "結帳"}}
    </div>
  </div>
  </div>
</template>

<script>
const dummyData = {
  cities: [
    "台北市",
    "基隆市",
    "新北市",
    "連江縣",
    "宜蘭縣",
    "新竹市",
    "新竹縣",
    "桃園市",
    "苗栗縣",
    "台中市",
    "彰化縣",
    "南投縣",
    "嘉義市",
    "嘉義縣",
    "雲林縣",
    "台南市",
    "高雄市",
    "澎湖縣",
    "金門縣",
    "屏東縣",
    "台東縣",
    "花蓮縣",
  ],
};
export default {
  props: {
    step: {
      type: Number,
      required: true,
    },
  },

  data() {
    return {
      checkout: {
        gender: "",
        name: "",
        tel: "",
        email: "",
        city: "",
        address: "",
        deliver: 0,
        payment: {
          name: "",
          cardNumber: "",
          expDate: "",
          cvc: ""
        }
      },
      cities: [],
      toggleStep: '',
    };
  },
  
  methods: {
    fetchCity() {
      this.cities = dummyData.cities.map((city) => ({
        ...city,
        name: city,
        id: dummyData.cities.indexOf(city),
      }));
    },
    handleDeliverChange() {
      this.$emit('afterDeliverChange', this.checkout.deliver)
    },
    handleSubmit(e) {
      const form = e.target;
      const formData = new FormData(form);
      this.$emit("after-submit", formData);
    },
    fetchStep() {
      this.toggleStep = this.step
    },

    togglePrevStep() {
      if(this.toggleStep > 0) {
        this.toggleStep -= 1
        this.handleStep()
      }
    },
    toggleNextStep() {
      if(this.toggleStep < 2) {
        this.toggleStep += 1
        this.handleStep()
      }else {
        this.handleStep
      }
      
    },
    handleStep() {
      this.$emit('after-stepToggle', this.toggleStep)
    }
  },
  created() {
    this.fetchCity();
    this.fetchStep();
  },
};
</script>