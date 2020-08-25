<template>
  <div class="mx-auto mt-2">
    <h2>Univers</h2>
    <p>{{ choice.universArray.join(", ") }}</p>
    <h2>Contraintes</h2>
    <p>{{ choice.contraintesArray.join(" - ") }}</p>
  </div>
</template>

<script>
export default {
  props: {
    choice: {
      type: Object,
    },
  },
};
</script>

<style></style>
