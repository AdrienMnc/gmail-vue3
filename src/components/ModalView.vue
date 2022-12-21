<template>
  <div class="modal">
    <div class="overlay" @click="emit('closeModal')"></div>
    <div class="modal-card">
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  setup(props, { emit }) {
    // let onKeydown = e => {
    //   if (e.key == "Escape") {
    //     emit("closeModal");
    //   }
    // };
    // window.addEventListener("keydown", onKeydown);
    // onBeforeUnmount(() => {
    //   window.removeEventListener("keydown", onKeydown);
    // });
    return {
      emit
    };
  }
};
</script>
