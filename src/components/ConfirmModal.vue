<template>
  <!-- Modal -->
  <div class="modal" id="modal-one">
    <div class="modal-dialog">
      <div class="modal-header">
        <h2>Confirmation</h2>
        <a href="#" class="btn-close" @click="onClose">×</a>
      </div>
      <div class="modal-body">
        <p>Do you want to delete this product?</p>
      </div>
      <div class="modal-footer">
        <a @click="onConfirm" href="#" class="btn">Delete</a>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'ConfirmModal',
  methods: {
    onClose(e) {
      e.preventDefault()
      this.$emit('closeModal')
    },
    onConfirm(e) {
      e.preventDefault()
      this.$emit('confirmRemoveItem')
    },
  },
}
</script>
<style scoped>
body {
  color: #333333;
  font-family: 'Helvetica', arial;
  height: 1500px;
}
.wrap {
  padding: 40px;
  text-align: center;
}
hr {
  clear: both;
  margin-top: 40px;
  margin-bottom: 40px;
  border: 0;
  border-top: 1px solid #aaaaaa;
}
h1 {
  font-size: 30px;
  margin-bottom: 40px;
}
p {
  margin-bottom: 20px;
}
.btn {
  background: #e99497;
  border-radius: 3px;
  color: #fff;
  display: inline-block;
  font-size: 14px;
  padding: 8px 15px;
  text-decoration: none;
  text-align: center;
  min-width: 60px;
  position: relative;
  transition: color 0.1s ease;
}
.btn:hover {
  background: #b3e283;
}
.btn.btn-big {
  font-size: 18px;
  padding: 15px 20px;
  min-width: 100px;
}
.btn-close {
  color: #aaaaaa;
  font-size: 30px;
  text-decoration: none;
  position: absolute;
  right: 5px;
  top: 0;
}
.btn-close:hover {
  color: #919191;
}
.modal:target:before {
  display: none;
}
.modal:before {
  content: '';
  display: block;
  background: rgba(0, 0, 0, 0.6);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 10;
}
.modal .modal-dialog {
  background: #fefefe;
  border-radius: 8px;
  margin-left: -200px;
  position: fixed;
  left: 50%;
  z-index: 11;
  min-width: 200px;
  width: 40%;
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  transform: translate(0, 0);
  -webkit-transition: -webkit-transform 0.3s ease-out;
  -moz-transition: -moz-transform 0.3s ease-out;
  -o-transition: -o-transform 0.3s ease-out;
  transition: transform 0.3s ease-out;
  top: 20%;
}
.modal:target .modal-dialog {
  top: -100%;
  -webkit-transform: translate(0, -500%);
  -ms-transform: translate(0, -500%);
  transform: translate(0, -500%);
}
.modal-body {
  padding: 20px;
}
.modal-header,
.modal-footer {
  padding: 10px 20px;
}
.modal-header {
  border-bottom: #eeeeee solid 1px;
}
.modal-header h2 {
  font-size: 20px;
}
.modal-footer {
  border-top: #eeeeee solid 1px;
  text-align: right;
}
</style>
