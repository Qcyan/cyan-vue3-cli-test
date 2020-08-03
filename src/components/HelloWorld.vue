<template>
  <div class="hello">
    <h4>This is an about page</h4>
    <div>
      {{title}},
      <button @click="changeTitle">点击换</button>
    </div>
    <slot />
    <slot name="cyanslot" />

    <button style="margin:20px" @click="TeleportFn">Teleport</button>
    <Teleport to="#foot-container">
      <div v-if="isOpen">
        <p>Bottom information bottom information bottom information bottom information bottom information bottom information</p>
        <button @click="isOpen = false">Cancel</button>
      </div>
    </Teleport>
  </div>
</template>

<script>
import { ref, reactive, toRefs, computed, watch } from "vue";
export default {
  name: "HelloWorld",
  props: {
    title: String,
  },
  setup() {
    const isOpen = ref(false);
    const msg = ref("this is a text");
    const changeTitle = function () {
      // this.$emit('update:foo', newValue)
    };
    function TeleportFn() {
      isOpen.value = true;
    }
    return {
      msg,
      isOpen,
      TeleportFn,
    };
  },
};
</script>


