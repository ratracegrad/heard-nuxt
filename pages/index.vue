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
/* --- End of TypeScript Types --- */

/* --- Variables --- */
const transactions = ref<Transaction[]>([])
const deleteOpen = ref(false)
const deleteTitle = ref('')
const deleteDescription = ref('')
const createOpen = ref(false)
const updateOpen = ref(false)
const transactionToUpdate = ref<Transaction>({
  id: 0,
  title: '',
  description: '',
  amount: 0,
  fromAccount: '',
  toAccount: '',
  transactionDate: '',
})
/* --- End of Variables --- */

/* --- Functions --- */
async function readTransactions() {
  const { data } = await supabase
    .from('transactions')
    .select(`id, title, description, amount, fromAccount, toAccount, transactionDate`)
    .order('id', { ascending: true })

  if (data)
    transactions.value = data
}

async function createTransaction(payload: Transaction) {
  const { error } = await supabase
    .from('transactions')
    .insert(payload)

  deleteTitle.value = error ? 'Error' : 'Transaction Created'
  deleteDescription.value = error ? error.message : 'Transaction Created Successfully!'
  createOpen.value = false
  deleteOpen.value = true
  readTransactions()
}

async function updateTransaction(payload: Transaction) {
  const { error } = await supabase
    .from('transactions')
    .update(payload)
    .eq('id', payload.id)

  deleteTitle.value = error ? 'Error' : 'Transaction Updated'
  deleteDescription.value = error ? error.message : 'Transaction Updated Successfully!'
  updateOpen.value = false
  deleteOpen.value = true
  readTransactions()
}

async function deleteTransaction(id: number) {
  const { error } = await supabase
    .from('transactions')
    .delete()
    .match({ id })

  deleteTitle.value = error ? 'Error' : 'Transaction Deleted'
  deleteDescription.value = error ? error.message : 'Transaction Deleted Successfully!'
  deleteOpen.value = true
  readTransactions()
}

function closeDeleteDialog() {
  deleteOpen.value = false
  readTransactions()
}

function closeCreateDialog() {
  createOpen.value = false
  readTransactions()
}

function showUpdate(transaction: Transaction) {
  transactionToUpdate.value = transaction
  updateOpen.value = true
}
function closeUpdateDialog() {
  updateOpen.value = false
  readTransactions()
}
/* --- End of Functions --- */

/* --- Lifecycle Hooks --- */
onMounted(() => {
  readTransactions()
})
/* --- End of Lifecycle Hooks --- */
</script>

<template>
  <div>
    <h2 class="mx-auto mt-10 text-center text-3xl text-gray-900 font-bold">
      Transactions List
    </h2>
    <div class="flex justify-end">
      <button class="mr-2 rounded bg-gray-300 px-4 py-2 text-black font-bold hover:bg-gray-500" @click="createOpen = true">
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
              <button class="mr-2 rounded bg-blue-500 px-4 py-2 text-white font-bold hover:bg-blue-700" @click="showUpdate(transaction)">
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

    <CreateModal
      :is-open="createOpen"
      @close-dialog="closeCreateDialog"
      @create-transaction="createTransaction"
    />
    <UpdateModal
      :is-open="updateOpen"
      :transaction="transactionToUpdate"
      @close-dialog="closeUpdateDialog"
      @update-transaction="updateTransaction"
    />
    <DeleteModal
      :is-open="deleteOpen"
      :title="deleteTitle"
      :description="deleteDescription"
      @close-dialog="closeDeleteDialog"
    />
  </div>
</template>
