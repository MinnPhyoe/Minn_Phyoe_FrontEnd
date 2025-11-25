<template>
  <div id="app">
    <!-- Navigation Bar -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
      <div class="container">
        <a class="navbar-brand" href="#">
          <i class="fas fa-graduation-cap"></i> After School Classes
        </a>
        <div class="ms-auto d-flex align-items-center">
          <!-- Search Bar -->
          <div class="me-3" v-if="showLessons">
            <input 
              type="text" 
              class="form-control" 
              placeholder="Search lessons..." 
              v-model="searchQuery"
              @input="searchLessons"
            >
          </div>
          
          <!-- Shopping Cart Button -->
          <button 
            class="btn btn-light position-relative" 
            @click="toggleCart"
            :disabled="cart.length === 0"
          >
            <i class="fas fa-shopping-cart"></i>
            <span 
              class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger" 
              v-if="cart.length > 0"
            >
              {{ cartItemCount }}
            </span>
          </button>
        </div>
      </div>
    </nav>

    <!-- Main Content -->
    <div class="container mt-4">
      <!-- Lessons Page -->
      <LessonsPage 
        v-if="showLessons"
        :sortedLessons="sortedLessons"
        :sortAttribute="sortAttribute"
        :sortOrder="sortOrder"
        @update:sortAttribute="sortAttribute = $event"
        @update:sortOrder="sortOrder = $event"
        @add-to-cart="addToCart"
      />

      <!-- Shopping Cart Page -->
      <CartPage 
        v-else
        :cart="cart"
        :totalPrice="totalPrice"
        :checkoutInfo="checkoutInfo"
        :isValidName="isValidName"
        :isValidPhone="isValidPhone"
        :canCheckout="canCheckout"
        :orderConfirmed="orderConfirmed"
        @update:name="checkoutInfo.name = $event"
        @update:phone="checkoutInfo.phone = $event"
        @remove-from-cart="removeFromCart"
        @checkout="checkout"
        @reset-order="resetOrder"
      />
    </div>
  </div>
</template>

<script>
import LessonsPage from './components/LessonsPage.vue'
import CartPage from './components/CartPage.vue'

export default {
  name: 'App',
  components: {
    LessonsPage,
    CartPage
  },
  data() {
    return {
      // API Base URL - REPLACE WITH YOUR RENDER URL
      apiUrl: 'https://minn-phyoe-coursework.onrender.com',
      
      // Lessons data
      lessons: [],
      filteredLessons: [],
      
      // Shopping cart
      cart: [],
      
      // UI state
      showLessons: true,
      orderConfirmed: false,
      
      // Sort options
      sortAttribute: 'subject',
      sortOrder: 'asc',
      
      // Search
      searchQuery: '',
      
      // Checkout information
      checkoutInfo: {
        name: '',
        phone: ''
      }
    }
  },
  computed: {
    // Sort lessons based on selected attribute and order
    sortedLessons() {
      const lessonsToSort = this.searchQuery ? this.filteredLessons : this.lessons
      
      return [...lessonsToSort].sort((a, b) => {
        let aVal = a[this.sortAttribute]
        let bVal = b[this.sortAttribute]
        
        // Convert to lowercase for string comparison
        if (typeof aVal === 'string') {
          aVal = aVal.toLowerCase()
          bVal = bVal.toLowerCase()
        }
        
        if (this.sortOrder === 'asc') {
          return aVal > bVal ? 1 : -1
        } else {
          return aVal < bVal ? 1 : -1
        }
      })
    },
    
    // Calculate total number of items in cart
    cartItemCount() {
      return this.cart.length
    },
    
    // Calculate total price
    totalPrice() {
      return this.cart.reduce((total, item) => total + item.price, 0)
    },
    
    // Validate name (letters only, including spaces)
    isValidName() {
      return /^[A-Za-z\s]+$/.test(this.checkoutInfo.name)
    },
    
    // Validate phone (numbers only)
    isValidPhone() {
      return /^[0-9]+$/.test(this.checkoutInfo.phone)
    },
    
    // Check if checkout button should be enabled
    canCheckout() {
      return this.isValidName && this.isValidPhone && this.cart.length > 0
    }
  },
  methods: {
    // Fetch all lessons from API
    async fetchLessons() {
      try {
        const response = await fetch(`${this.apiUrl}/lessons`)
        if (!response.ok) {
          throw new Error('Failed to fetch lessons')
        }
        this.lessons = await response.json()
        console.log('Lessons loaded:', this.lessons)
      } catch (error) {
        console.error('Error fetching lessons:', error)
        alert('Failed to load lessons. Please check if the server is running.')
      }
    },
    
    // Search lessons (Challenge Component - Search as you type)
    async searchLessons() {
      if (!this.searchQuery.trim()) {
        this.filteredLessons = []
        return
      }
      
      try {
        const response = await fetch(
          `${this.apiUrl}/search?q=${encodeURIComponent(this.searchQuery)}`
        )
        if (!response.ok) {
          throw new Error('Search failed')
        }
        this.filteredLessons = await response.json()
        console.log('Search results:', this.filteredLessons)
      } catch (error) {
        console.error('Error searching lessons:', error)
      }
    },
    
    // Add lesson to cart
    addToCart(lesson) {
      if (lesson.space > 0) {
        // Add to cart
        this.cart.push({...lesson})
        
        // Decrease space locally (will be updated in DB after checkout)
        lesson.space--
        
        console.log('Added to cart:', lesson)
      }
    },
    
    // Remove lesson from cart
    removeFromCart(index) {
      const removedItem = this.cart[index]
      
      // Find the lesson in the lessons array and increase space
      const lesson = this.lessons.find(l => l._id === removedItem._id)
      if (lesson) {
        lesson.space++
      }
      
      // Remove from cart
      this.cart.splice(index, 1)
      console.log('Removed from cart:', removedItem)
    },
    
    // Toggle between lessons page and cart page
    toggleCart() {
      this.showLessons = !this.showLessons
      
      // Reset order confirmation when going back to cart
      if (!this.showLessons) {
        this.orderConfirmed = false
      }
    },
    
    // Checkout and submit order
    async checkout() {
      if (!this.canCheckout) {
        return
      }
      
      try {
        // Prepare order data
        const lessonIDs = this.cart.map(item => item._id)
        const numberOfSpaces = this.cart.reduce((acc, item) => {
          acc[item._id] = (acc[item._id] || 0) + 1
          return acc
        }, {})
        
        const orderData = {
          name: this.checkoutInfo.name,
          phone: this.checkoutInfo.phone,
          lessonIDs: lessonIDs,
          numberOfSpaces: numberOfSpaces
        }
        
        console.log('Submitting order:', orderData)
        
        // Submit order via POST
        const orderResponse = await fetch(`${this.apiUrl}/orders`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(orderData)
        })
        
        if (!orderResponse.ok) {
          throw new Error('Failed to submit order')
        }
        
        const orderResult = await orderResponse.json()
        console.log('Order submitted:', orderResult)
        
        // Update lesson spaces in database via PUT
        await this.updateLessonSpaces()
        
        // Show confirmation
        this.orderConfirmed = true
        
      } catch (error) {
        console.error('Error during checkout:', error)
        alert('Failed to submit order. Please try again.')
      }
    },
    
    // Update lesson spaces in database after order
    async updateLessonSpaces() {
      // Count how many times each lesson appears in cart
      const lessonUpdates = {}
      this.cart.forEach(item => {
        if (!lessonUpdates[item._id]) {
          lessonUpdates[item._id] = {
            id: item._id,
            currentSpace: this.lessons.find(l => l._id === item._id).space
          }
        }
      })
      
      // Update each lesson's space in database
      for (const lessonId in lessonUpdates) {
        try {
          const response = await fetch(`${this.apiUrl}/lessons/${lessonId}`, {
            method: 'PUT',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              space: lessonUpdates[lessonId].currentSpace
            })
          })
          
          if (!response.ok) {
            throw new Error('Failed to update lesson space')
          }
          
          console.log(`Updated lesson ${lessonId}`)
        } catch (error) {
          console.error(`Error updating lesson ${lessonId}:`, error)
        }
      }
    },
    
    // Reset order and go back to lessons page
    resetOrder() {
      this.cart = []
      this.checkoutInfo = { name: '', phone: '' }
      this.orderConfirmed = false
      this.showLessons = true
      
      // Refresh lessons from server
      this.fetchLessons()
    }
  },
  mounted() {
    this.fetchLessons()
  }
}
</script>

<style>
/* Custom Styles */
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f8f9fa;
}

.navbar {
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.navbar-brand {
  font-size: 1.5rem;
  font-weight: bold;
}

.btn-light:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.badge {
  font-size: 0.7rem;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .navbar-brand {
    font-size: 1.2rem;
  }
}
</style>
