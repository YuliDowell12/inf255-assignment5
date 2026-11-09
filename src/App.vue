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

      <!-- TODO 4: The AnimalControls component goes here. -->

      <div id="animal-list">
        <p v-if="loading">Loading animals...</p>
        <p v-else-if="error" class="error-message">{{ error }}</p>
        <div v-else>
          <AnimalCard
            v-for="animal in animals"
            :key="animal.id"
            :animal="animal"
            @toggle="toggleAdoption(animal)"
          />
        </div>
      </div>
    </section>
  </main>
</template>
