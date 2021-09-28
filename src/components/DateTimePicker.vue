<template>
  <a-popconfirm
    okText="确定"
    cancelText="取消"
    @visibleChange="handlePopconfirmVisibleChange"
    @confirm="handlePopconfirmConfirm"
  >
    <a-input placeholder="Basic usage" />
    <span slot="icon" />
    <div class="wrapper" slot="title">
      <div :id="date.id" ref="date" />
      <div :id="time.id" ref="time" />
    </div>
  </a-popconfirm>
</template>

<script>
import laydate from 'layui-laydate';
import dayjs from 'dayjs';
import objectSupport from 'dayjs/plugin/objectSupport';
import { customAlphabet } from 'nanoid';

dayjs.extend(objectSupport);

export default {
  name: 'DateTimePicker',

  props: {
    // 初始值，可以是 String 或者 Date 对象
    value: {
      type: [String, Date],
      default() {
        return dayjs()
          .startOf('date')
          .toDate();
      },
    },
    // confirm 事件参数值的格式化占位符
    // 注意，这里是 dayjs 里面的占位符标准，具体查看 https://dayjs.gitee.io/docs/zh-CN/display/format
    format: {
      type: String,
      default: 'YYYY-MM-DD HH:mm:ss',
    },
  },

  data() {
    return {
      isRender: false,
      date: {
        id: this.generateId(),
        type: 'date',
        value: null,
      },
      time: {
        id: this.generateId(),
        type: 'time',
        value: null,
      },
    };
  },

  created() {
    // 因为 laydate 是动态加载 css，这里需要做一个特殊处理，步骤如下：
    // 在 node_modules 中找到 laydate/theme，将其拷贝到 public/laydate 目录下
    // 最终目录应为 public/laydate/theme/default/...
    laydate.path = '/laydate/';
  },

  methods: {
    handlePopconfirmConfirm() {
      this.$emit('confirm', this.getDateTime());
    },
    handlePopconfirmVisibleChange(visible) {
      if (this.isRender) {
        return;
      }

      this.$nextTick(() => {
        this.renderLayDate(this.date);
        this.renderLayDate(this.time, 'time');
      });
      this.isRender = true;
    },

    getDateTime() {
      const { year, month, date } = this.date.value;
      const { hours, minutes, seconds } = this.time.value;
      const datetime = { year, month, date, hours, minutes, seconds };

      return dayjs(datetime).format(this.format);
    },
    renderLayDate(options) {
      const value = dayjs(this.value)
        .clone()
        .toDate();

      laydate.render({
        elem: document.getElementById(options.id),
        type: options.type,
        value: value,
        btns: ['now'],
        position: 'static',
        ready: (date) => {
          this[options.type].value = date;
        },
        change: (value, date) => {
          this[options.type].value = date;
        },
        done: (value, date) => {
          this[options.type].value = date;
        },
      });
    },
    generateId() {
      return customAlphabet('abcdefghijklmnopqrstuvwxyz0123456789-_', 5)();
    },
  },
};
</script>

<style scoped>
.wrapper {
  width: 555px;
  display: flex;
  justify-content: space-between;
}
</style>
