<template>
  <div class="popup_wraper" ref="popup_wrapper">
    <div class="popup">
      <div class="popup__header">
        <span>Look out!</span>
        <span class="close_popup" vertical-align="middle" @click="closePopup">×</span>
      </div>
      <div class="popup__content">
        <slot></slot>
      </div>
      <div class="popup__footer">
        <button class="close_modal" @click="closePopup">Close</button>
        <button class="submit_btn" @click="submitPopup">Ok</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Popup',
  props: {},
  data() {
    return {}
  },
  methods: {
    closePopup() {
      this.$emit('close-popup')
    },
    submitPopup() {
      this.$emit('submit-popup')
    }
  },
  mounted() {
    let vm = this
    document.addEventListener('click', (event) => {
      if (event.target === vm.$refs['popup_wrapper']) {
        vm.closePopup()
      }
    })
  }
}
</script>

<style scoped>
.popup_wraper {
  display: flex;
  background: #5a5a5a8a;
  justify-content: center;
  align-items: center;
  position: absolute;
  right: 0;
  bottom: 0;
  left: 0;
  top: 0;
  z-index: 10;
}

.popup {
  padding: 6px 16px 16px 16px;
  position: fixed;
  top: 50px;
  width: 400px;
  background: #fff;
  box-shadow: 0 0 17px 0 #00000033;
  border-radius: 4px;
  z-index: 10;
}

.popup__header, .popup__footer {
  font-weight: bold;
  font-size: 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.popup__content {
  display: flex;
  justify-content: center;
  align-items: center;
}

.submit_btn {
  padding: 10px;
  color: #fff;
  background: #26ae68;
  border: none;
  border-radius: 3px;
}

.submit_btn:focus {
  outline-style: none;
  outline-width: 0px !important;
  outline-color: none !important;
}

.close_modal {
  padding: 10px;
  color: #fff;
  background:  red;
    border: none;
  border-radius: 3px;
}

.close_modal:focus {
  outline-style: none;
  outline-width: 0px !important;
  outline-color: none !important;
}

.close_popup{
  font-weight: bolder;
  color: red;
  font-size: 2.5rem;
}
</style>