<template>
  <div class="alert alert-success mt-4 order-confirmation">
    <h4 class="alert-heading">
      <i class="fas fa-check-circle"></i> Order Confirmed!
    </h4>
    <p>Your order has been successfully submitted. We'll contact you shortly.</p>
    <hr>
    <button class="btn btn-success" @click="$emit('reset')">
      Continue Shopping
    </button>
  </div>
</template>

<script>
export default {
  name: 'OrderConfirmation',
  emits: ['reset']
}
</script>

<style scoped>
.order-confirmation {
  animation: fadeIn 0.5s ease-in;
  border-radius: 0.5rem;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
