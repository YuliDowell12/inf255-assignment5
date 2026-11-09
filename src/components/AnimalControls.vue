<script setup>
// TODO 4: Declare the activeFilter prop and the filter and sort events.
defineProps({
  activeFilter: String,
});

const emit = defineEmits(["filter", "sort"]);

// Write onSortChange(event).

function onSortChange(event) {
  emit("sort", event.target.value);
}
</script>

<template>
  <div class="controls">
    <div class="filters" role="group" aria-label="Filter animals">
      <button
        type="button"
        :class="{ active: activeFilter === 'all' }"
        @click="emit('filter', 'all')"
      >
        All
      </button>
      <button
        type="button"
        :class="{ active: activeFilter === 'available' }"
        @click="emit('filter', 'available')"
      >
        Available
      </button>
      <button
        type="button"
        :class="{ active: activeFilter === 'adopted' }"
        @click="emit('filter', 'adopted')"
      >
        Adopted
      </button>
    </div>

    <label for="sort-animals">Sort by:</label>
    <select id="sort-animals" @change="onSortChange">
      <option value="default">Original order</option>
      <option value="name">Name</option>
      <option value="age">Age</option>
    </select>
  </div>
</template>
