<template>
  <div class="checkout-page">
    <h2 class="mb-4">Shopping Cart</h2>

    <!-- Cart Items -->
    <div v-if="cart.length > 0">
      <!-- Cart Item Cards -->
      <div class="card mb-3 cart-item-card" v-for="(item, index) in cart" :key="index">
        <div class="card-body">
          <div class="row align-items-center">
            <div class="col-md-2 text-center">
              <i :class="'fas ' + item.icon + ' fa-2x text-primary'"></i>
            </div>
            <div class="col-md-6">
              <h5>{{ item.subject }}</h5>
              <p class="mb-0">
                <i class="fas fa-map-marker-alt"></i> {{ item.location }}
              </p>
            </div>
            <div class="col-md-2">
              <strong>£{{ item.price }}</strong>
            </div>
            <div class="col-md-2">
              <button class="btn btn-danger btn-sm" @click="$emit('remove-from-cart', index)">
                <i class="fas fa-trash"></i> Remove
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Total Price -->
      <div class="card bg-light">
        <div class="card-body">
          <h4 class="text-end">Total: £{{ totalPrice }}</h4>
        </div>
      </div>

      <!-- Checkout Form -->
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
    </div>

    <!-- Empty Cart Message -->
    <div v-else class="alert alert-info">
      <i class="fas fa-shopping-cart"></i> Your cart is empty. Add some lessons to get started!
    </div>

    <!-- Order Confirmation -->
    <div v-if="orderConfirmed" class="alert alert-success mt-4 order-confirmation">
      <h4 class="alert-heading">
        <i class="fas fa-check-circle"></i> Order Confirmed!
      </h4>
      <p>Your order has been successfully submitted. We'll contact you shortly.</p>
      <hr>
      <button class="btn btn-success" @click="$emit('reset-order')">
        Continue Shopping
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CheckoutPage',
  props: {
    cart: {
      type: Array,
      required: true
    },
    totalPrice: {
      type: Number,
      required: true
    },
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
    },
    orderConfirmed: {
      type: Boolean,
      required: true
    }
  },
  emits: ['update:name', 'update:phone', 'remove-from-cart', 'checkout', 'reset-order']
}
</script>

<style scoped>
.cart-item-card {
  transition: transform 0.2s;
}

.cart-item-card:hover {
  transform: translateX(5px);
}

.is-valid {
  border-color: #28a745;
}

.is-invalid {
  border-color: #dc3545;
}

.form-control:focus {
  border-color: #0d6efd;
  box-shadow: 0 0 0 0.2rem rgba(13, 110, 253, 0.25);
}

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
