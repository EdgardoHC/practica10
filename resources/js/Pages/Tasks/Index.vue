<script setup>
import { ref } from 'vue'
import { useForm } from '@inertiajs/vue3'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'

const props = defineProps({ tasks: Array })
const form = useForm({ title: '' })

function submit() {
  form.post(route('tasks.store'), { preserveScroll: true })
}
</script>

<template>
  <AuthenticatedLayout title="Lista de Tareas">
    <div class="max-w-2xl mx-auto p-4">
      <form @submit.prevent="submit" class="mb-4 flex gap-2">
        <input v-model="form.title" class="flex-1 border p-2 rounded" placeholder="Nueva tarea" />
        <button class="bg-emerald-600 hover:bg-emerald-700 text-white px-4 py-2 rounded shadow">
          Agregar
        </button>

      </form>

      <ul>
        <li v-for="task in tasks" :key="task.id" class="flex justify-between items-center border-b py-2">
          <label class="flex items-center gap-2">
            <input type="checkbox" :checked="task.completed"
              @change="$inertia.put(route('tasks.update', task.id), { completed: !task.completed })" />
            <span :class="{ 'line-through text-gray-500': task.completed }">{{ task.title }}</span>
          </label>
          <button @click="$inertia.delete(route('tasks.destroy', task.id))" class="text-red-500">Eliminar</button>
        </li>
      </ul>
    </div>
  </AuthenticatedLayout>
</template>
