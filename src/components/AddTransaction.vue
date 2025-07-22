<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");

const emit = defineEmits(["transactionSubmitted"]);
const toast = useToast();

const handleSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("All fields need to be filled");
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  text.value = "";
  amount.value = "";
  emit("transactionSubmitted", transactionData);
};
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">Amount</label>
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
