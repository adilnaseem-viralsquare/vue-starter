<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        id="text"
        v-model="title"
        placeholder="Enter text..."
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="number"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";
const title = ref("");
const amount = ref("");
const toast = useToast();
// this is our custom event emmitter, when it is emited, we can listen for this specific emit anywhere
const emit = defineEmits(["transactionSubmitted"]);

const onSubmit = () => {
  if (!title.value || !amount.value) {
    toast.error("Both fields are required");
    return;
  }
  const transactionData = { text: title.value, amount: amount.value };
  emit("transactionSubmitted", transactionData);
  title.value = "";
  amount.value = "";
};
</script>
