<template>
  <div class="wrapper-popup" v-bind:class="getClassPopup">
    <div class="rule">
      <h3>Luật Chơi</h3>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Rem reiciendis cupiditate dignissimos unde nostrum ea praesentium officia quasi possimus veritatis voluptate quia assumenda quos optio modi perferendis, quod deserunt eaque!</p>
      <button class="confirm" v-on:click="confirm">Đã Hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup-rule",
  props: {
    isOpenPopup: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {};
  },
  methods:{
    confirm(){
      this.$emit('handleConfirm')
    }
  },
  computed: {
    getClassPopup() {
      return {
        'open-popup': this.isOpenPopup
      }
    }
  }
};
</script>

<style>
.wrapper-popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  /* background-color: #ccc; */
  background-color: rgba(0, 0, 0, 0.4);
  opacity: 0;
  visibility: hidden;
  transition: all .3s ease;
}
.wrapper-popup.open-popup {
  opacity: 1;
  visibility: visible;
}
.rule {
  width: 350px;
  padding: 20px;
  background-color: white;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%) scale(1.2);
  position: absolute;
  transition: all .3s ease;
}
.open-popup .rule {
  transform: translate(-50%,-50%) scale(1);
}
.rule h3 {
  margin-bottom: 10px;
}
.rule .confirm {
  cursor: pointer;
  margin-top: 20px;
  border: 2px solid #333;
  background-color: white;
  padding: 8px 15px;
  transition: all .3s ease;
}
.rule .confirm:hover {
  color: white;
  background-color: #333;
}
/* zoom in & zoom out */
</style>