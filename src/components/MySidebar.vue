<script setup lang="ts">
const props = defineProps(['active_note', 'notes']);
</script>

<template>
  <aside class="w-[256px] min-h-screen bg-gray-800">
    <div class="p-4">
      <button class="button w-full" @click="$emit('new-note')">New Note</button>
    </div>

    <ul>
      <li
        v-for="note in props.notes"
        :key="note.id"
        @click="$emit('set-active-note', note.id)"
        class="p-4 border-b border-gray-700 hover:bg-gray-600 cursor-pointer flex justify-between items-center gap-4"
      >
        <span> {{ note.title }}</span>
        <button
          @click="(evt) => $emit('delete-note', note.id, evt)"
          class="text-red-500 cursor-pointer"
        >
          Delete
        </button>
      </li>
    </ul>
  </aside>
</template>
