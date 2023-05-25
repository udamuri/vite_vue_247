<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})

const count = ref(0);
const numbers = ref();
const small_number = ref();
const int_number = ref(0);
const messages = ref();
const disabled_tabs = ref();

disabled_tabs.value = {
  tab1 : {nav: 'active',content: 'tab-pane show active'}, 
  tab2: {nav: 'disabled',content: 'tab-pane fade'}, 
  tab3: {nav: 'disabled',content: 'tab-pane fade'},
};
numbers.value = [];

function generateCount() {
  messages.value = null;
  if (isNaN(count.value)) {
    messages.value = {generate: "Please enter Numeric value"};
  } else {
    let count_flor = Math.floor(count.value);
    if (count_flor > 0 && count_flor <= 10000) {
      disabled_tabs.value ={tab1 : 'disabled', tab2: 'active', tab3: ''};
      disabled_tabs.value = {
        tab1 : {nav: 'disabled',content: 'tab-pane fade'}, 
        tab2: {nav: 'active',content: 'tab-pane show active'}, 
        tab3: {nav: 'disabled',content: 'tab-pane fade'},
      };
    } else {
      messages.value = {generate: "Please enter Numeric value from 1 to 10000"};
    }
  }
}

function generateTabe() {
  let count_flor = Math.floor(count.value);
  let positive_integer = 11111;
  numbers.value = [];
  for(let i = 0;i< count_flor;i++) {
    let rand_int = getRndInteger(-10000, 10000);
    if(rand_int > 0 ) {
      if(positive_integer > rand_int) {
        positive_integer = rand_int;
      }
    }
    numbers.value.push(rand_int);
  }

  small_number.value = positive_integer;
  int_number.value = 0;
  disabled_tabs.value = {
    tab1 : {nav: 'disabled',content: 'tab-pane fade'}, 
    tab2: {nav: 'active',content: 'tab-pane show active'}, 
    tab3: {nav: '',content: 'tab-pane fade'},
  };
}

function integerNumber() {
  int_number.value = 0;
  if(small_number.value !== null && small_number.value !== 11111) {
    let intnumber = small_number.value
    if(small_number.value > 1) {
      int_number.value = intnumber - 1;
    } else {
      int_number.value = "";
    }
  } else {
    int_number.value = 1;
  }
}

function clearData() {
  count.value = 0;
  numbers.value = null;
  small_number.value = 11111;
  int_number.value = 0;
  messages.value = null;
  disabled_tabs.value = {
    tab1 : {nav: 'active',content: 'tab-pane show active'}, 
    tab2: {nav: 'disabled',content: 'tab-pane fade'}, 
    tab3: {nav: 'disabled',content: 'tab-pane fade'},
  };

  console.log(disabled_tabs.value);
}

function getRndInteger(min, max) {
  return Math.floor(Math.random() * (max - min) ) + min;
}

</script>

<template>
  <section class="py-5 text-center container" >
    <ul class="nav nav-tabs" id="myTab" role="tablist">
      <li class="nav-item" role="presentation">
        <button class="nav-link" :class="disabled_tabs?.tab1?.nav" id="home-tab" data-bs-toggle="tab" data-bs-target="#home" type="button" role="tab" aria-controls="home" aria-selected="true">Page 1</button>
      </li>
      <li class="nav-item" role="presentation">
        <button class="nav-link" :class="disabled_tabs?.tab2?.nav" id="profile-tab" data-bs-toggle="tab" data-bs-target="#profile" type="button" role="tab" aria-controls="profile" aria-selected="false">Page 2</button>
      </li>
      <li class="nav-item" role="presentation">
        <button class="nav-link" :class="disabled_tabs?.tab3?.nav" id="contact-tab" data-bs-toggle="tab" data-bs-target="#contact" type="button" role="tab" aria-controls="contact" aria-selected="false">Page 3</button>
      </li>
    </ul>

    <div class="tab-content" id="myTabContent">
      <div :class="disabled_tabs?.tab1?.content" id="home" role="tabpanel" aria-labelledby="home-tab">
        <div class="row g-3 mt-5">
          <div class="col-auto">
            <label for="staticEmail2" class="">Gererate From 1 to 10000</label>
          </div>
          <div class="col-auto">
            <label for="generate" class="visually-hidden">Password</label>
            <input type="text" class="form-control" id="generate" v-model="count" placeholder="1 to 10000">
            <div class="text-danger text-sm mt-2" v-if="messages?.generate">
              {{ messages?.generate }}
            </div>
          </div>
          <div class="col-auto">
            <button type="button" class="btn btn-primary mb-3" @click="generateCount()">Generate</button>
          </div>
        </div>
      </div>
      <div :class="disabled_tabs?.tab2?.content" id="profile" role="tabpanel" aria-labelledby="profile-tab">
        <div><br/><br/></div>
        <div class="table-responsive">
          <div class="col-auto">
            <button type="button" class="btn btn-primary mb-3" @click="generateTabe()">Generate</button>&nbsp;
            <button type="button" class="btn btn-primary mb-3" @click="clearData()">Clear</button>
          </div>

          <table class="table caption-top">
            <caption>Data</caption>
            <thead>
              <tr>
                <th scope="col" width="1%">#</th>
                <th scope="col">Numbers</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in numbers" :key="index">
                <th scope="row">{{ index + 1 }}</th>
                <td class="text-start">{{ item }}</td>
              </tr>
              
            </tbody>
          </table>
        </div>

      </div>
      <div :class="disabled_tabs?.tab3?.content" id="contact" role="tabpanel" aria-labelledby="contact-tab">
        <div><br/><br/></div>
        <div class="table-responsive">
          <div class="col-auto">
            <button type="button" class="btn btn-primary mb-3" @click="integerNumber()">Generate</button>
          </div>

          <table class="table caption-top" v-if="int_number">
            <caption>Bilangan Bulat Positive</caption>
            <tbody>
              <tr>
                <td class="text-start">{{ int_number }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
</style>
