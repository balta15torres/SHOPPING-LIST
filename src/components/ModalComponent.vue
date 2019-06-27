<template>
  <div class="ModalComponent">
    <div class="ModalComponent__poput">
      <p>{{text}}</p>
      <div class="ModalComponent__poput--btns">
      <slot name="actions"></slot>      
      </div>
    </div>
  </div>
</template>
 
 <script>
export default {
  name: "ModalComponent",

  props: {
    text: {
      type: String
    }
  }
};
</script>
 <style lang="scss">
.ModalComponent {
  align-items: center;
  background-color: rgba(53, 73, 94, 0.5);
  bottom: 0;
  display: flex;
  justify-content: space-evenly;
  left: 0;
  position: fixed;
  right: 0;
  top: 0;
  z-index: 10;

  .ModalComponent__poput {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 0 8px 0 rgb(255, 255, 255);
    width: 400px;
    height: 100px;
    background-color: rgb(66, 185, 131);
    p{
      font-size: 18px;
      padding-right: 20px;
    }
    .ModalComponent__poput--btns {
      button {
        margin: 8px;
        width: 35px;
        height: 35px;
      }
    }
  }
}
</style>
 