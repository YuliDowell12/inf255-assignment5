<script setup>
/*
 * Animal Adoption Board
 *
 * EXTRA CREDIT: If you recorded the video walkthrough (see README.md), paste
 * its link here. A video that is not linked here will not be graded.
 *
 *   Video link:
 */

import { ref, computed } from "vue";
import AnimalCard from "./components/AnimalCard.vue";
import AnimalControls from "./components/AnimalControls.vue";

// ------------------------------------
// App Data

const animals = ref([]);
const loading = ref(false);
const error = ref("");

// ------------------------------------

// TODO 1: Create the app's data with ref(): the animals array, loading,
// and error.

// ------------------------------------
// Load the Animals

async function loadAnimals() {
  loading.value = true;
  error.value = "";

  try {
    const response = await fetch("/animals.json");

    if (!response.ok) {
      throw new Error(`Request failed with status ${response.status}`);
    }

    animals.value = await response.json();
  } catch (err) {
    console.error(err);
    error.value =
      "Sorry, the animals could not be loaded. Try refreshing the page.";
  }

  loading.value = false;
}

// ------------------------------------

// TODO 1: Write loadAnimals() and call it.

loadAnimals();

// ------------------------------------
// Adopt or Return an Animal
// ------------------------------------

// TODO 3: Write toggleAdoption(animal).

function toggleAdoption(animal) {
  animal.adopted = !animal.adopted;
}

// ------------------------------------
// Filter, Sort, and Count
// ------------------------------------

// TODO 4: Create activeFilter and activeSort, the visibleAnimals and
// availableCount computed properties, and setFilter() and setSort().

const activeFilter = ref("all");
const activeSort = ref("default");

// the visibleAnimals computed property replaced updateDisplay(). Instead of filtering, sorting, and drawing cards by hand,
// it works out which animals should be shown and returns that list. Vue notices which refs the code reads,
// so whichever one of them changes (activeFilter, activeSort, animals), Vue updates the page automatically. So,
// updatDisplay() doesnt have to be called again after anything is updated.

const visibleAnimals = computed(() => {
  // Work done on a copy so sorting never reorders the animals array itself.
  let visible = animals.value.slice();

  if (activeFilter.value === "available") {
    visible = visible.filter((a) => !a.adopted);
  } else if (activeFilter.value === "adopted") {
    visible = visible.filter((a) => a.adopted);
  }

  if (activeSort.value === "name") {
    visible.sort((a, b) => a.name.localeCompare(b.name));
  } else if (activeSort.value === "age") {
    // Negative result puts a before b, so smaller ages come first.
    visible.sort((a, b) => a.age - b.age);
  }

  return visible; // returned, not drawn, the template does the drawing
});

const availableCount = computed(() => {
  return animals.value.filter((a) => !a.adopted).length;
});

function setFilter(filter) {
  activeFilter.value = filter;
}

function setSort(sort) {
  activeSort.value = sort;
}
</script>

<template>
  <header>
    <h1>Animal Adoption Board</h1>
    <p>Meet the animals currently staying at our shelter.</p>
  </header>

  <main>
    <section aria-labelledby="animals-heading">
      <h2 id="animals-heading">Animals</h2>

      <!-- TODO 4: The available count goes here. -->

      <p v-if="!loading && !error" id="animal-count">
        {{ availableCount }} of {{ animals.length }} animals are available for
        adoption.
      </p>

      <!-- TODO 4: The AnimalControls component goes here. -->

      <AnimalControls
        :active-filter="activeFilter"
        @filter="setFilter"
        @sort="setSort"
      />

      <div id="animal-list">
        <p v-if="loading">Loading animals...</p>
        <p v-else-if="error" class="error-message">{{ error }}</p>
        <p v-else-if="visibleAnimals.length === 0" class="empty-message">
          There are no animals to show.
        </p>
        <div v-else>
          <AnimalCard
            v-for="animal in visibleAnimals"
            :key="animal.id"
            :animal="animal"
            @toggle="toggleAdoption(animal)"
          />
        </div>
      </div>
    </section>
  </main>
</template>
