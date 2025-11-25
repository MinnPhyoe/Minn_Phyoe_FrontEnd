<template>
  <div class="card mt-4">
    <div class="card-header bg-primary text-white">
      <h5 class="mb-0">Checkout Information</h5>
    </div>
    <div class="card-body">
      <div class="mb-3">
        <label class="form-label">Name (letters only):</label>
        <input 
          type="text" 
          class="form-control" 
          :value="checkoutInfo.name"
          @input="$emit('update:name', $event.target.value)"
          :class="{
            'is-invalid': checkoutInfo.name && !isValidName, 
            'is-valid': isValidName
          }"
          placeholder="Enter your full name"
        >
        <div class="invalid-feedback" v-if="checkoutInfo.name && !isValidName">
          Name must contain letters only
        </div>
      </div>
      <div class="mb-3">
        <label class="form-label">Phone (numbers only):</label>
        <input 
          type="text" 
          class="form-control" 
          :value="checkoutInfo.phone"
          @input="$emit('update:phone', $event.target.value)"
          :class="{
            'is-invalid': checkoutInfo.phone && !isValidPhone, 
            'is-valid': isValidPhone
          }"
          placeholder="Enter your phone number"
        >
        <div class="invalid-feedback" v-if="checkoutInfo.phone && !isValidPhone">
          Phone must contain numbers only
        </div>
      </div>
      <button 
        class="btn btn-success w-100" 
        @click="$emit('checkout')"
        :disabled="!canCheckout"
      >
        <i class="fas fa-check"></i> Complete Order
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CheckoutForm',
  props: {
    checkoutInfo: {
      type: Object,
      required: true
    },
    isValidName: {
      type: Boolean,
      required: true
    },
    isValidPhone: {
      type: Boolean,
      required: true
    },
    canCheckout: {
      type: Boolean,
      required: true
    }
  },
  emits: ['update:name', 'update:phone', 'checkout']
}
</script>

<style scoped>
.is-valid {
  border-color: #28a745;
}

.is-invalid {
  border-color: #dc3545;
}

.form-control:focus {
  border-color: #0d6efd;
  box-shadow: 0 0 0 0.2rem rgba(13,110,253,0.25);
}
</style>
