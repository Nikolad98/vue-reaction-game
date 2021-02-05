<template>
  <div class="screen">
      <div class="breakInformations">
          <slot></slot>
      </div>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>

.screen{
    position: absolute;
    width: 100vw;
    height: 100vh;
    top: 0;
    left: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.5);
}

</style>