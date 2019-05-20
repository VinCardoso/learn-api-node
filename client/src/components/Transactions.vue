<template>
    <v-card style="width: 100%;">

        <!-- Card Title -->
        <v-card-title>
            <span class="pr-3">
                Extrato de {{ months[currentMonth].name }} - {{ currentYear }}
            </span>

            <v-btn flat icon class="pr-2" v-on:click="gotoMonth(-1)">
                <v-icon>keyboard_arrow_left</v-icon>
            </v-btn>

            <v-btn flat icon class="pr-2" v-on:click="gotoCurrentMonth">
                <v-icon>today</v-icon>
            </v-btn>

            <v-btn flat icon v-on:click="gotoMonth(1)">
                <v-icon>keyboard_arrow_right</v-icon>
            </v-btn>

            <v-spacer></v-spacer>

            <!-- Search -->
            <v-text-field
                append-icon="search"
                label="Pesquisar"
                single-line
                hide-details
                v-model="search"
            ></v-text-field>

        </v-card-title>

        <!-- Table -->
        <v-data-table
            v-bind:headers="headers"
            v-bind:items="items"
            v-bind:search="search"
            item-key="_id">
            <template slot="items" slot-scope="props">
                <tr>
                    <td>
                        <v-edit-dialog lazy>
                            {{ props.item.transactionDate }}
                        <v-text-field
                            slot="input"
                            label="Edit"
                            v-model="props.item.transactionDate"
                            single-line></v-text-field>
                        </v-edit-dialog>
                    </td>
                <td class="text-xs-left">{{ props.item.transactionType }}</td>
                <td class="text-xs-left" @click="props.expanded = !props.expanded">{{ props.item.description }}</td>
                <td class="text-xs-right">{{ props.item.charge }}</td>
                <td class="text-xs-right">{{ props.item.deposit }}</td>
                <td class="text-xs-right">{{ props.item.balance }}</td>
            </tr>
            </template>

            <template slot="expand" slot-scope="props">
                <v-card flat class="pl-5">
                <v-card-text class="text-xs-left">
                    <v-text-field label="Note" v-model="props.item.notes"></v-text-field>
                </v-card-text>
                </v-card>
            </template>

            <template slot="pageText" slot-scope="{ pageStart, pageStop }">
                De {{ pageStart }} à {{ pageStop }}
            </template>

            <template slot="rows-per-page-items" slot-scope="{ pageStart, pageStop }">
                De {{ pageStart }} à {{ pageStop }}
            </template>

        </v-data-table>
    </v-card>
</template>

<script>
export default {
  computed: {
    currentMonth () {
      return this.$store.state.transactions.currentMonth
    },
    currentYear () {
      return this.$store.state.transactions.currentYear
    },
    months () {
      return this.$store.state.transactions.months
    },
    items () {
      return this.$store.getters.transactionsByMonth
    },
    balanceCharges () {
      return this.$store.getters.balanceCharges
    },
    balanceDeposits () {
      return this.$store.getters.balanceDeposits
    }
  },
  data () {
    return {
      max25chars: (v) => v.length <= 25 || 'Input too long!',
      search: '',
      pagination: {},
      headers: [
        { text: 'Data', align: 'center', sortable: false, value: 'date' },
        { text: 'Tipo', align: 'center', sortable: false, value: 'type' },
        { text: 'Descrição', align: 'center', sortable: false, value: 'description' },
        { text: 'Gasto (-)', align: 'center', sortable: false, value: 'paymentAmt' },
        { text: 'Depósito (+)', align: 'center', sortable: false, value: 'depositAmt' },
        { text: 'Saldo', align: 'center', sortable: false, value: 'balance' }
      ]
      // items: [
      //   {
      //     '_id': '5a8245999f63531c3ce288bc',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '1 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288ba',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '2 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288bd',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '3 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288bf',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '4 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288bh',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '5 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288b1',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '6 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288b2',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '7 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288b3',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '8 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288b4',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '9 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288b5',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '10 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288b6',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '11 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288b7',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '12 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   },
      //   {
      //     '_id': '5a8245999f63531c3ce288b8',
      //     'userId': '5a777f0a75f64a1690221d98',
      //     'transactionDate': 'May-15',
      //     'transactionType': 'Reward',
      //     'description': '13 New account first charge reward deposit',
      //     'charge': 0.0,
      //     'deposit': 100.0,
      //     'notes': ''
      //   }
      // ]
    }
  },

  methods: {
    getTransactionsByMonth: function () {
      this.$store.dispatch('getTransactionsByMonth')
    },
    getPreviousMonthsBalances: function () {
      this.$store.dispatch('getPreviousMonthsBalances')
    },
    // mapTransactions: function (tx) {
    //   const me = this
    //   const transDate = new Date(tx.transactionDate)
    //   let transaction = {
    //     transactionDate: me.months[transDate.getUTCMonth() + 1].abrev + '-' + transDate,
    //     transactionType: tx.transactionType,
    //     description: tx.description,
    //     charge: me.moneyFormatter(tx.charge),
    //     deposit: me.moneyFormatter(tx.deposit),
    //     balance: me.moneyFormatter(me.calcRunningBalance(tx))
    //   }
    //   return transaction
    // },
    // moneyFormatter: function (amount) {
    //   let formatter = new Intl.NumberFormat('pt-BR', {
    //     style: 'currency',
    //     currency: 'BRL',
    //     minimumFractionDigits: 2
    //   })
    //   return formatter.format(amount)
    // },
    // calcRunningBalance: function (tx) {
    //   //   if (tx && tx.charge > 0){
    //   //       this.balanceCharges += tx.charge
    //   //   }else if(tx && tx.deposit)
    //   //   }
    //   return tx
    // }
    gotoMonth: function (increment) {
      this.$store.dispatch('gotoMonth').then(() => {
        this.getPreviousMonthsBalances()
        this.getTransactionsByMonth()
      })
    },
    gotoCurrentMonth: function () {
      this.$store.dispatch('gotoCurrentMonth').then(() => {
        // Load selected month transaction data now..
        this.getPreviousMonthsBalances()
        this.getTransactionsByMonth()
      })
    }
  },
  mounted: async function () {
    await this.getPreviousMonthsBalances()
    await console.log(this.balanceCharges)
    await console.log(this.balanceDeposits)

    await this.getTransactionsByMonth()
  }

}
</script>

<style>

</style>
