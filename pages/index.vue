<script setup lang="ts">
const supabase = useSupabaseClient()

async function getTransactions() {
  const { data } = await supabase
    .from('transactions')
    .select(`id, title, description, amount, fromAccount, toAccount, transactionDate`)
    .order('id', { ascending: true })

  if (data) 
    console.log('data', data)
  
}

onMounted(() => {
  getTransactions()
})


</script>

<template>
  <div>
    <h1>Hello World</h1>
  </div>
</template>
