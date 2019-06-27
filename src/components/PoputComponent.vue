<template>
  <div class="PoputComponent">
    <div class="PoputComponent__poput">
      <h1>{{text}}</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "PoputComponent",

  props: {
    text: {
      type: String
    }
  }
};
</script>

<style lang="scss">
.PoputComponent {
  display: flex;
  align-items: center;
  background-color: rgba(53, 73, 94, 0.5);
  bottom: 0;
  justify-content: space-evenly;
  border-radius: 12px;
  right: 0;
  top: 0;
  margin:20px;
  .PoputComponent__poput{
    
    width: 80%;
    height: 50%;
    background-color: #42b983;
    border-radius: 12px;
    margin-bottom: 20px;
    margin-top: 20px;
    display: flex;
    justify-content: center;
  align-items: center;
  padding: 20px;
  box-shadow: 0 0 8px 0 rgb(255, 255, 255);
  }
}
</style>
