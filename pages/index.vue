<script setup lang="ts">
const supabase = useSupabaseClient()

/* --- TypeScript Types --- */
interface Transaction {
  id: number
  title: string
  description: string
  amount: number
  fromAccount: string
  toAccount: string
  transactionDate: string
}

/* --- Variables --- */
const loading = ref(true)
const transactions = ref<Transaction[]>([])

async function getTransactions() {
  const { data } = await supabase
    .from('transactions')
    .select(`id, title, description, amount, fromAccount, toAccount, transactionDate`)
    .order('id', { ascending: true })

  if (data)
    transactions.value = data
}

function addTransaction() {
  console.log('button clicked')
}

function editTransaction(id: number) {
  console.log('button clicked', id)
}

function deleteTransaction(id: number) {
  console.log('button clicked', id)
}

onMounted(() => {
  getTransactions()
})
</script>

<template>
  <div>
    <h2 class="mx-auto mt-10 text-center text-3xl text-gray-900 font-bold">
      Transactions List
    </h2>
    <div class="flex justify-end">
      <button class="mr-2 rounded bg-gray-300 px-4 py-2 text-black font-bold hover:bg-gray-500" @click="addTransaction()">
        + Add
      </button>
    </div>
    <div>
      <table class="mx-auto mt-10 border-collapse table-auto border border-gray-400">
        <thead>
          <tr>
            <th class="border border-gray-400 px-4 py-2">
              ID
            </th>
            <th class="border border-gray-400 px-4 py-2">
              Title
            </th>
            <th class="border border-gray-400 px-4 py-2">
              Description
            </th>
            <th class="border border-gray-400 px-4 py-2">
              Amount
            </th>
            <th class="border border-gray-400 px-4 py-2">
              From Account
            </th>
            <th class="border border-gray-400 px-4 py-2">
              To Account
            </th>
            <th class="border border-gray-400 px-4 py-2">
              Transaction Date
            </th>
            <th class="border border-gray-400 px-4 py-2">
              Actions
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="transaction in transactions" :key="transaction.id">
            <td class="border border-gray-400 px-4 py-2">
              {{ transaction.id }}
            </td>
            <td class="border border-gray-400 px-4 py-2">
              {{ transaction.title }}
            </td>
            <td class="border border-gray-400 px-4 py-2">
              {{ transaction.description }}
            </td>
            <td class="border border-gray-400 px-4 py-2">
              {{ transaction.amount }}
            </td>
            <td class="border border-gray-400 px-4 py-2">
              {{ transaction.fromAccount }}
            </td>
            <td class="border border-gray-400 px-4 py-2">
              {{ transaction.toAccount }}
            </td>
            <td class="border border-gray-400 px-4 py-2">
              {{ transaction.transactionDate }}
            </td>
            <td class="border border-gray-400 px-4 py-2">
              <button class="mr-2 rounded bg-blue-500 px-4 py-2 text-white font-bold hover:bg-blue-700" @click="editTransaction(transaction.id)">
                Edit
              </button>
              <button class="rounded bg-red-500 px-4 py-2 text-white font-bold hover:bg-red-700" @click="deleteTransaction(transaction.id)">
                Remove
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
