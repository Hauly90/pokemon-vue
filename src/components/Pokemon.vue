<template>
  <div>
    <h3>Hello From Component</h3>
    <h5>Received name: {{ name }}</h5>
  </div>
</template>

<script>
export default {
  name: "Pokemon",

  data() {
    return {
      formName: [],
    };
  },

  props: {
    name: String,
  },
};
</script>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
