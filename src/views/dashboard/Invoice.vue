<template>
  <div class="page-invoices">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Invoice - {{ invoice.invoice_number }}</h1>
      </div>
      <div class="column is-12">
        <h3 class="is-size-4">Client</h3>
        <p>
          <strong>{{ invoice.client_name }}</strong>
        </p>
        <p v-if="invoice.client_address1">{{ invoice.client_address1 }}</p>
        <p v-if="invoice.client_address2">{{ invoice.client_address2 }}</p>
        <p v-if="invoice.client_zipcode || invoice.client_province">
          {{ invoice.client_zipcode }} {{ invoice.client_province }}
        </p>
        <p v-if="invoice.client_country">{{ invoice.client_country }}</p>
      </div>
      <div class="column is-12">
        <h3 class="is-size-4">Items</h3>

        <table class="table is-fullwidth">
          <thead>
            <tr>
              <td>#</td>
              <td>Title</td>
              <td>Quantity</td>
              <td>Amount</td>
            </tr>
          </thead>

          <tbody>
            <tr class="item" v-for="item in items" v-bind:key="item.id">
              <td>{{ item.id }}</td>
              <td>{{ item.title }}</td>
              <td>{{ item.quantity }}</td>
              <td>{{ item.net_amount }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Invoice",
  data() {
    return {
      invoice: {},
      items: [],
    };
  },
  async mounted() {
    await this.getInvoice();
    await this.getItems();
  },
  methods: {
    getInvoice() {
      const invoiceID = this.$route.params.id;
      console.log(invoiceID);
      axios
        .get(`api/v1/invoices/${invoiceID}`)
        .then((response) => {
          console.log(response);
          this.invoice = response.data;
        })
        .catch((error) => {
          console.log(JSON.stringify(error));
        });
    },
    getItems() {
      const invoiceID = this.$route.params.id;

      axios
        .get(`/api/v1/items/?invoice_id=${invoiceID}`)
        .then((response) => {
          this.items = response.data;
        })
        .catch((error) => {
          console.log(JSON.stringify(error));
        });
    },
  },
};
</script>
