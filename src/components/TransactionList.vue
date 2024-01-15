<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      v-bind:key="transaction.id"
      v-bind:class="transaction.amount > 0 ? 'plus' : 'minus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn" @click="() => handleDelete(transaction.id)">
        x
      </button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from "vue";
const emit = defineEmits(["deleteTransaction"]);
// export default {
// // options-api
//   data() {
//     return {
//       transactions: [
//         { id: 1, text: "Flowers", amount: -19.99 },
//         { id: 2, text: "Salary", amount: +400 },
//         { id: 3, text: "Books", amount: -10.99 },
//         { id: 4, text: "Camera", amount: 150 },
//       ],
//     };
//   },
// <-----------------------older way to use composition api----------------------------->
//
/**
 *
 setup() {
     const transactions = [
         { id: 1, text: "Flowers", amount: -19.99 },
         { id: 2, text: "Salary", amount: +400 },
         { id: 3, text: "Books", amount: -10.99 },
      { id: 4, text: "Camera", amount: 150 },
    ];
    // myfunction(){}
    return {
        transactions,

        //   myfunction,
    };
},
*/

// };

// <-----------------------newer way to use composition api----------------------------->
// just put setup keyword in script tag and no need to create a setup function
// and manually return object and use export
// just define your variables and you are good to go

// const transactions = [
//   { id: 1, text: "Flowers", amount: -19.99 },
//   { id: 2, text: "Salary", amount: +400 },
//   { id: 3, text: "Books", amount: -10.99 },
//   { id: 4, text: "Camera", amount: 150 },
// ];

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const handleDelete = function (id) {
  emit("deleteTransaction", id);
};
</script>
