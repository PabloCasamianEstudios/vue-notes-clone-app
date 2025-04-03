<script setup lang="ts">
import { ref, nextTick } from 'vue';
import MySidebar from './components/MySidebar.vue';

interface Note {
  id: number;
  title: string;
  body: string;
}

const notes = ref<Note[]>([]);

const active_note = ref<number | null>(null);

const input_title = ref('');
const input_body = ref('');

// create note
const createNote = async () => {
  const id = Date.now();
  notes.value.push({
    id,
    title: 'untitled',
    body: '',
  });

  await nextTick();
  setActiveNote(id);
};

// set active note

const setActiveNote = async (id: number) => {
  active_note.value = id;
  const note = notes.value.find((note) => note.id === id);

  if (note) {
    input_title.value = note.title;
    input_body.value = note.body;
  } else {
    input_title.value = '';
    input_body.value = '';
  }

  await nextTick();
  const inputElement = document.querySelector('input[type=text]') as HTMLInputElement;
  inputElement?.focus();
};

// delete note

const deleteNote = (id: number, event: MouseEvent) => {
  event.stopPropagation();

  const index = notes.value.findIndex((note) => note.id === id);
  if (index !== -1) {
    notes.value.splice(index, 1); // Elimina la nota del array
  }

  // Si la nota eliminada era la activa, limpia la vista
  if (active_note.value === id) {
    active_note.value = null;
    input_title.value = '';
    input_body.value = '';
  }
};

// update note

const updateNote = () => {
  const note = notes.value.findIndex((note) => note.id === active_note.value);

  notes.value[note].title = input_title.value;
  notes.value[note].body = input_body.value;
};
</script>

<template>
  <div class="bg-gray-700 text-white min-h-screen flex items-stretch justify-start">
    <!-- Sidebar -->
    <MySidebar
      :active_note="active_note"
      :notes="notes"
      @new-note="createNote"
      @set-active-note="setActiveNote"
      @delete-note="deleteNote"
    />

    <!-- Main Content -->
    <main class="flex-1">
      <div v-if="active_note" class="px-4 py-8 flex flex-col h-full">
        <input
          v-model="input_title"
          @input="updateNote"
          type="text"
          class="block w-full text-3xl pb-2 font-bold border-b-2 border-gray-500 focus:border-white outline-none transition-colors duration-200"
        />
        <textarea
          v-model="input_body"
          @input="updateNote"
          placeholder="Write your note here..."
          class="block w-full h-full mt-4 text-lg outline-none flex-1"
        ></textarea>
      </div>
    </main>
  </div>
</template>
