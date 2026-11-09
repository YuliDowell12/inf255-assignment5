<script setup>
import dayjs from "dayjs";
// TODO 2: Declare the animal prop with defineProps().
defineProps({
  animal: Object,
});

function arrivalText(intakeDate) {
  const arrived = dayjs(intakeDate);
  return `Arrived ${arrived.format("MMM D, YYYY")} (${dayjs().diff(arrived, "day")} days ago)`;
}
// TODO 3: Declare the toggle event with defineEmits().
const emit = defineEmits(["toggle"]);
</script>

<template>
  <!-- TODO 2: Build one animal's card. See README.md for the markup. -->
  <article class="animal-card" :class="{ adopted: animal.adopted }">
    <h3>{{ animal.name }}</h3>
    <p>{{ animal.species }}, {{ animal.age }} years old</p>
    <p>{{ arrivalText(animal.intakeDate) }}</p>
    <p>{{ animal.adopted ? "Adopted" : "Available" }}</p>
    <button type="button" @click="emit('toggle')">
      {{ animal.adopted ? "Return" : "Adopt" }}
    </button>
  </article>
</template>
