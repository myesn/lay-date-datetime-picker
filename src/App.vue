<template>
  <div id="app">
    <DateTimePicker @confirm="handleConfirm" />
  </div>
</template>

<script>
import DateTimePicker from './components/DateTimePicker.vue';

export default {
  name: 'App',
  components: {
    DateTimePicker,
  },
  methods: {
    handleConfirm(dateTime) {
      console.log(`handleConfirm: ${dateTime}`);
    },
  },
};
</script>
