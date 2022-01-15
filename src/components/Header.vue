<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homePage"
      :text="formShown ? 'Close Form' : 'Add Task'"
      :color="formShown ? 'red' : 'green'"
      @click="$emit('toggle-form')"
    />
  </header>
</template>

<script>
import Button from './Button';
export default {
  name: 'Header',
  //   props: ['title'],
  components: {
    Button,
  },
  props: {
    // title: String,
    title: {
      default: 'Hello World',
      type: String,
    },
    formShown: Boolean,
  },
  computed: {
    homePage() {
      //bu componentin verilen yerde gözükmemesi için yazdığımız kural
      if (this.$route.path === '/') {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
