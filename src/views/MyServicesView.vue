<template>
  <div class="wrapper">
    <h2 class="text-gray-600 text-2xl font-semibold mb-4 mt-4 text-center">{{ $t('My Services') }}</h2>

    <ServiceForm v-if="showForm" :service="selectedService" @close="closeForm" @saved="loadServices" />

    <div class="text-right">
      <button @click="createNew"class="mb-4 px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700">{{ $t('Add new service') }}</button>
    </div>

    <div class="overflow-x-auto">
      <div v-if="loading" class="text-center py-10 text-gray-500">{{ $t('Loading services...') }}</div>
      <table v-if="!loading" class="min-w-full bg-white border border-gray-200 rounded-lg shadow">
        <thead>
          <tr class="bg-gray-100 text-left text-sm uppercase text-gray-600">
            <th class="px-4 py-3">{{ $t('Name') }}</th>
            <th class="px-4 py-3">{{ $t('Price (PLN)') }}</th>
            <th class="px-4 py-3 text-right"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="s in services" :key="s.id" class="border-t hover:bg-gray-50">
            <td class="px-4 py-2 text-gray-600 font-medium">{{ s.name }}</td>
            <td class="px-4 py-2 text-gray-600">{{ s.price }}</td>
            <td class="px-4 py-2 space-x-2 text-right">
              <button @click="editService(s)" class="px-3 py-1 bg-yellow-400 text-white rounded hover:bg-yellow-500">{{ $t('Edit') }}</button>
              <button @click="deleteService(s.id)" class="px-3 py-1 bg-red-500 text-white rounded hover:bg-red-600">{{ $t('Delete') }}</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import api from '@/services/api'
import ServiceForm from '@/components/ServiceForm.vue'

const services = ref([])
const selectedService = ref(null)
const showForm = ref(false)
const loading = ref(false)

const loadServices = async () => {
  loading.value = true
  try {
    const res = await api.get('/services')
    services.value = res.data
  } finally {
    loading.value = false
  }
}

const createNew = () => {
  selectedService.value = null
  showForm.value = true
}

const editService = (s) => {
  selectedService.value = s
  showForm.value = true
}

const deleteService = async (id) => {
  if (confirm('Na pewno chcesz usunąć?')) {
    await api.delete(`/services/${id}`)
    loadServices()
  }
}

const closeForm = () => {
  showForm.value = false
}

onMounted(() => {
  loadServices()
})
</script>
