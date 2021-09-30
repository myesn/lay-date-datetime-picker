<template>
  <div id="app">
    <p>开始日期：{{start}}</p>
    <p>结束日期：{{end}}</p>
    <DateTimePicker v-model="start" />
    <DateTimePicker v-model="end" />
  </div>
</template>

<script>
import DateTimePicker from './components/DateTimePicker.vue';

export default {
  name: 'App',
  components: {
    DateTimePicker,
  },

  data(){
    return {
      start: null,
      end: null
    }
  },

};
</script>
