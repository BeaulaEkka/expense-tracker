<template>
  <h3>Add new Transction</h3>
  <form action="" id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        id="text"
        placeholder="Enter Text ..."
        v-model="text"
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        placeholder="Enter Amount ..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add Transaction</button>
  </form>
</template>


<script setup>
import { ref } from "vue";
import { useToast } from 'vue-toastification';

const text = ref("");
const amount = ref("");
const emit=defineEmits(['transationsSubmitted'])

const toast=useToast();

const onSubmit = () => {
if(!text.value || !amount.value){
toast.error('Both fields must be filled')
return;}

const transactionData={
    text:text.value,
    amount:parseFloat(amount.value),
}
emit('transactionSubmitted',transactionData);
  text.value='';
  amount.value-'';
};
</script>
