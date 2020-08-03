<template>
  <div>132</div>
</template>



<script>
import { ref, reactive, toRefs, computed, watch } from "vue";
export default {
  setup() {
    const msg = ref("this is a text");
    return {
      msg,
    };
  },
};
</script>
