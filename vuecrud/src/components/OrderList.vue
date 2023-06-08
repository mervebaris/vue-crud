<template>
 <div>
   <h1>Order List</h1>
   <table>
     <thead>
       <tr>
         <th>ID</th>
         <th>Order Date</th>
         <th>Description</th>
         <th>Payment Type</th>
         <th>Order Details</th>
         <th>Customer Name</th>
         <th></th>
       </tr>
     </thead>
     <tbody>
       <tr v-for="order in orders" :key="order.id">
         <td>{{ order.id }}</td>
         <td>{{ order.orderDate }}</td>
         <td>{{ order.description }}</td>
         <td>{{ order.paymentType }}</td><td>{{ Array.isArray(order.orderDetails) ? order.orderDetails.join(', ') : '' }}</td>

         <td>{{ order.customerName }}</td>
         <td>
           <button @click="editOrder(order)">Edit</button>
           <button @click="deleteOrder(order.id)">Delete</button>
         </td>
       </tr>
     </tbody>
   </table>
   <form v-if="showForm" @submit.prevent="saveOrder">
     <h2 v-if="isEditing">Edit Order</h2>
     <h2 v-else>Add Order</h2>
     <div>
       <label for="orderId">ID:</label>
       <input type="text" id="orderId" v-model="newOrder.id" :disabled="isEditing" required>
     </div>
     <div>
       <label for="orderDate">Order Date:</label>
       <input type="date" id="orderDate" v-model="newOrder.orderDate" required>
     </div>
     <div>
       <label for="description">Description:</label>
       <input type="text" id="description" v-model="newOrder.description" required>
     </div>
     <div>
       <label for="paymentType">Payment Type:</label>
       <input type="text" id="paymentType" v-model="newOrder.paymentType" required>
     </div>
     <div>
       <label for="orderDetails">Order Details:</label>
       <input type="text" id="orderDetails" v-model="newOrder.orderDetails" required>
     </div>
     <div>
       <label for="customerName">Customer Name:</label>
       <input type="text" id="customerName" v-model="newOrder.customerName" required>
     </div>
     <div>
       <button type="submit">{{ isEditing ? 'Save' : 'Add' }}</button>
       <button type="button" @click="cancelEdit">Cancel</button>
     </div>
   </form>
   <button @click="addOrder">Add Order</button>
 </div>
</template>

<script>
export default {
  data() {
    return {
      orders: [
        {
          "id": 1,
          "orderDate": "2023-06-02",
          "description": "sample description",
          "paymentType": "Kredi Kartı",
          "orderDetails": [
            "productId1",
            "productId2",
            "productId3"
          ],
          "customerName": "Ofiscom Yazılım"
        }
      ],
      newOrder: {},
      showForm: false,
      isEditing: false
    };
  },
 methods: {
   addOrder() {
     this.showForm = true;
     this.isEditing = false;
     this.newOrder = {
       id: '',
       orderDate: '',
       description: '',
       paymentType: '',
       orderDetails: '',
       customerName: ''
     };
   },
   saveOrder() {
     if (this.isEditing) {
       // Update existing order
       const index = this.orders.findIndex(order => order.id === this.newOrder.id);
       if (index !== -1) {
         this.orders.splice(index, 1, this.newOrder);
       }
     } else {
       // Add new order
       this.orders.push(this.newOrder);
     }
     this.cancelEdit();
   },
   editOrder(order) {
     this.showForm = true;
     this.isEditing = true;
     this.newOrder = Object.assign({}, order);
   },
   deleteOrder(id) {
     const index = this.orders.findIndex(order => order.id === id);
     if (index !== -1) {
       this.orders.splice(index, 1);
     }
   },
   cancelEdit() {
     this.showForm = false;
     this.isEditing = false;
     this.newOrder = {};
   }},
 
  computed: {
    formatOrderDetails() {
      return orderDetails => orderDetails.join(', ');
    }
  }
};
</script>

<style scoped>
table {
 width: 100%;
 border-collapse: collapse;
}

th, td {
 padding: 8px;
 text-align: left;
 border-bottom: 1px solid #ddd;
}

button {
 margin: 5px;
}
</style>
