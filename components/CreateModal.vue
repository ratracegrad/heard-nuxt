<script setup lang="ts">
import {
  Dialog,
  DialogPanel,
  DialogTitle,
  TransitionChild,
  TransitionRoot,
} from '@headlessui/vue'

const props = defineProps({
  isOpen: Boolean,
})

const title = ref('')
const description = ref('')
const amount = ref(0)
const fromAccount = ref('')
const toAccount = ref('')
const transactionDate = new Intl.DateTimeFormat('en-US').format(new Date())

const emit = defineEmits(['closeDialog', 'createTransaction'])

function closeDialog() {
  emit('closeDialog')
}

function createTransaction() {
  emit('createTransaction', {
    title: title.value,
    description: description.value,
    amount: amount.value,
    fromAccount: fromAccount.value,
    toAccount: toAccount.value,
    transactionDate,
  })
}
</script>

<template>
  <TransitionRoot as="template" :show="props.isOpen">
    <Dialog as="div" class="relative z-10" @close="closeDialog">
      <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
      </TransitionChild>

      <div class="fixed inset-0 z-10 w-screen overflow-y-auto">
        <div class="min-h-full flex items-end justify-center p-4 text-center sm:items-center sm:p-0">
          <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95" enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200" leave-from="opacity-100 translate-y-0 sm:scale-100" leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
            <DialogPanel class="relative transform overflow-hidden rounded-lg bg-white px-4 pb-4 pt-5 text-left shadow-xl transition-all sm:my-8 sm:max-w-sm sm:w-full sm:p-6">
              <div>
                <div class="mt-3 text-center sm:mt-5">
                  <DialogTitle as="h3" class="text-base text-gray-900 font-semibold leading-6">
                    Create Transaction
                  </DialogTitle>
                </div>

                <div class="grid grid-cols-1 mt-4 gap-x-6 gap-y-4 sm:grid-cols-6">
                  <div class="col-span-full">
                    <label for="title" class="block text-sm text-gray-900 font-medium leading-6">Title</label>
                    <div class="mt-2">
                      <input v-model="title" id="title" type="text" name="title" autocomplete="title" class="block w-full border-0 rounded-md py-1.5 text-gray-900 shadow-sm ring-1 ring-gray-300 ring-inset sm:text-sm placeholder:text-gray-400 sm:leading-6 focus:ring-2 focus:ring-gray-600 focus:ring-inset">
                    </div>
                  </div>
                  <div class="col-span-full">
                    <label for="description" class="block text-sm text-gray-900 font-medium leading-6">Description</label>
                    <div class="mt-2">
                      <input v-model="description" id="description" type="text" name="description" autocomplete="description" class="block w-full border-0 rounded-md py-1.5 text-gray-900 shadow-sm ring-1 ring-gray-300 ring-inset sm:text-sm placeholder:text-gray-400 sm:leading-6 focus:ring-2 focus:ring-gray-600 focus:ring-inset">
                    </div>
                  </div>
                  <div class="col-span-full">
                    <label for="amount" class="block text-sm text-gray-900 font-medium leading-6">Amount</label>
                    <div class="mt-2">
                      <input v-model="amount" id="amount" type="number" name="amount" autocomplete="amount" class="block w-full border-0 rounded-md py-1.5 text-gray-900 shadow-sm ring-1 ring-gray-300 ring-inset sm:text-sm placeholder:text-gray-400 sm:leading-6 focus:ring-2 focus:ring-gray-600 focus:ring-inset">
                    </div>
                  </div>
                  <div class="col-span-full">
                    <label for="fromAccount" class="block text-sm text-gray-900 font-medium leading-6">From Account</label>
                    <div class="mt-2">
                      <input v-model="fromAccount" id="fromAccount" type="text" name="fromAccount" autocomplete="fromAccount" class="block w-full border-0 rounded-md py-1.5 text-gray-900 shadow-sm ring-1 ring-gray-300 ring-inset sm:text-sm placeholder:text-gray-400 sm:leading-6 focus:ring-2 focus:ring-gray-600 focus:ring-inset">
                    </div>
                  </div>
                  <div class="col-span-full">
                    <label for="toAccount" class="block text-sm text-gray-900 font-medium leading-6">To Account</label>
                    <div class="mt-2">
                      <input v-model="toAccount" id="toAccount" type="text" name="toAccount" autocomplete="toAccount" class="block w-full border-0 rounded-md py-1.5 text-gray-900 shadow-sm ring-1 ring-gray-300 ring-inset sm:text-sm placeholder:text-gray-400 sm:leading-6 focus:ring-2 focus:ring-gray-600 focus:ring-inset">
                    </div>
                  </div>
                </div>
              </div>
              <div class="mt-6 flex items-center justify-end gap-x-6">
                <button type="button" class="text-sm text-gray-900 font-semibold leading-6" @click="closeDialog">
                  Close
                </button>
                <button type="submit" class="rounded-md bg-blue-600 px-3 py-2 text-sm text-white font-semibold shadow-sm hover:bg-blue-500 focus-visible:outline-2 focus-visible:outline-blue-600 focus-visible:outline-offset-2 focus-visible:outline" @click="createTransaction">
                  Create
                </button>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<style scoped>

</style>
