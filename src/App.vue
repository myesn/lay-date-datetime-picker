<template>
  <div id="app">
    <p>开始日期：{{start}}</p>
    <p>结束日期：{{end}}</p>
    <DateTimePicker placeholder="请选择开始时间" v-model="start" />
    <DateTimePicker placeholder="请选择结束时间" v-model="end" />
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
