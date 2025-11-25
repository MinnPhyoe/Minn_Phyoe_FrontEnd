<template>
  <div>
    <h2 class="mb-4">Shopping Cart</h2>
    
    <!-- Cart Items -->
    <div v-if="cart.length > 0">
      <CartItem
        v-for="(item, index) in cart"
        :key="index"
        :item="item"
        :index="index"
        @remove="$emit('remove-from-cart', index)"
      />

      <!-- Total Price -->
      <div class="card bg-light">
        <div class="card-body">
          <h4 class="text-end">Total: £{{ totalPrice }}</h4>
        </div>
      </div>

      <!-- Checkout Form -->
      <CheckoutForm
        :checkoutInfo="checkoutInfo"
        :isValidName="isValidName"
        :isValidPhone="isValidPhone"
        :canCheckout="canCheckout"
        @update:name="$emit('update:name', $event)"
        @update:phone="$emit('update:phone', $event)"
        @checkout="$emit('checkout')"
      />
    </div>

    <!-- Empty Cart Message -->
    <div v-else class="alert alert-info">
      <i class="fas fa-shopping-cart"></i> Your cart is empty. Add some lessons to get started!
    </div>

    <!-- Order Confirmation Modal -->
    <OrderConfirmation
      v-if="orderConfirmed"
      @reset="$emit('reset-order')"
    />
  </div>
</template>

<script>
import CartItem from './CartItem.vue'
import CheckoutForm from './CheckoutForm.vue'
import OrderConfirmation from './OrderConfirmation.vue'

export default {
  name: 'CartPage',
  components: {
    CartItem,
    CheckoutForm,
    OrderConfirmation
  },
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
