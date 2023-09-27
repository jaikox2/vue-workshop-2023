<script setup>
import { computed, onMounted, ref } from 'vue'
import axios from 'axios'

const products = ref([])

function totalCost(cost, quantity) {
  return cost * quantity
}

const allTotalCost = computed(() => {
  return products.value.reduce((total, product) => {
    return (total += product.cost * product.quantity)
  }, 0)
})

function loadProducts() {
  axios.get('https://api.budu.triple-i.in/products').then(function (response) {
    // handle success
    products.value = response.data
  })
}

onMounted(() => {
  loadProducts()
})

function onDelete(id) {
  axios.delete(`https://api.budu.triple-i.in/products/${id}`).then(function (response) {
    loadProducts()
  })
}
</script>

<template>
  <main>
    <!-- Component: Simple with footer -->
    <div class="w-full overflow-x-auto">
      <table
        class="w-full text-left border border-separate rounded border-slate-200"
        cellspacing="0"
      >
        <tbody>
          <tr>
            <th
              scope="col"
              class="h-12 px-6 text-sm font-medium text-center border-l first:border-l-0 stroke-slate-700 text-slate-700 bg-slate-100"
            >
              No
            </th>
            <th
              scope="col"
              class="h-12 px-6 text-sm font-medium border-l first:border-l-0 stroke-slate-700 text-slate-700 bg-slate-100"
            >
              Product Name
            </th>
            <th
              scope="col"
              class="h-12 px-6 text-sm font-medium border-l first:border-l-0 stroke-slate-700 text-slate-700 bg-slate-100"
            >
              Price
            </th>
            <th
              scope="col"
              class="h-12 px-6 text-sm font-medium border-l first:border-l-0 stroke-slate-700 text-slate-700 bg-slate-100"
            >
              Cost
            </th>
            <th
              scope="col"
              class="h-12 px-6 text-sm font-medium border-l first:border-l-0 stroke-slate-700 text-slate-700 bg-slate-100"
            >
              Quantity
            </th>
            <th
              scope="col"
              class="h-12 px-6 text-sm font-medium border-l first:border-l-0 stroke-slate-700 text-slate-700 bg-slate-100"
            >
              Total Cost
            </th>
            <th
              scope="col"
              class="h-12 px-6 text-sm font-medium border-l first:border-l-0 stroke-slate-700 text-slate-700 bg-slate-100"
            ></th>
          </tr>
          <tr v-for="product in products" :key="product.id">
            <th
              scope="row"
              class="h-12 px-6 text-sm text-center transition duration-300 border-t border-l first:border-l-0 border-slate-200 stroke-slate-500 text-slate-500"
            >
              {{ product.id }}
            </th>
            <td
              class="h-12 px-6 text-sm transition duration-300 border-t border-l first:border-l-0 border-slate-200 stroke-slate-500 text-slate-500"
            >
              {{ product.name }}
            </td>
            <td
              class="h-12 px-6 text-sm transition duration-300 border-t border-l first:border-l-0 border-slate-200 stroke-slate-500 text-slate-500"
            >
              {{ product.price }}
            </td>
            <td
              class="h-12 px-6 text-sm transition duration-300 border-t border-l first:border-l-0 border-slate-200 stroke-slate-500 text-slate-500"
            >
              {{ product.cost }}
            </td>
            <td
              class="h-12 px-6 text-sm transition duration-300 border-t border-l first:border-l-0 border-slate-200 stroke-slate-500 text-slate-500"
            >
              {{ product.quantity }}
            </td>
            <td
              class="h-12 px-6 text-sm transition duration-300 border-t border-l first:border-l-0 border-slate-200 stroke-slate-500 text-slate-500"
            >
              {{ totalCost(product.cost, product.quantity) }}
            </td>
            <td
              class="h-12 px-6 text-sm transition duration-300 border-t border-l first:border-l-0 border-slate-200 stroke-slate-500 text-slate-500"
            >
              <RouterLink :to="`/edit/${product.id}`">
                <!-- Component: Small primary basic button -->
                <button
                  class="inline-flex items-center justify-center h-8 gap-2 px-4 text-xs font-medium tracking-wide text-white transition duration-300 rounded focus-visible:outline-none whitespace-nowrap bg-blue-500 hover:bg-blue-600 focus:bg-blue-700 disabled:cursor-not-allowed disabled:border-blue-300 disabled:bg-blue-300 disabled:shadow-none"
                >
                  <span>Edit</span>
                </button>
              </RouterLink>
              <!-- End Small primary basic button -->
              <button
                @click="onDelete(product.id)"
                class="ml-2 inline-flex items-center justify-center h-8 gap-2 px-4 text-xs font-medium tracking-wide text-white transition duration-300 rounded focus-visible:outline-none whitespace-nowrap bg-red-500 hover:bg-red-600 focus:bg-red-700 disabled:cursor-not-allowed disabled:border-red-300 disabled:bg-red-300 disabled:shadow-none"
              >
                <span>Delete</span>
              </button>
            </td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <td
              class="h-12 px-6 text-sm font-medium border-t border-l first:border-l-0 stroke-slate-700 text-slate-700"
              colspan="4"
            ></td>
            <td
              class="h-12 px-6 text-sm font-medium border-t border-l first:border-l-0 stroke-slate-700 text-slate-700"
            >
              Total Cost
            </td>
            <td
              class="h-12 px-6 text-sm font-medium border-t border-l first:border-l-0 stroke-slate-700 text-slate-700"
              colspan="2"
            >
              {{ allTotalCost }}
            </td>
          </tr>
        </tfoot>
      </table>
    </div>
    <!-- End Simple with footer -->
  </main>
</template>
