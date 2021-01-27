<template>
  <div class="header">
    <b-nav tabs>
      <b-nav-item disabled>Fancy Quiz App</b-nav-item>
      <b-nav-item disabled
        >Counter <small>(10 Questions)</small> : {{ correctAnswers }}/{{
          totalAnswers
        }}</b-nav-item
      >
    </b-nav>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    correctAnswers: Number,
    totalAnswers: Number,
  },
};
</script>
