<template>
  <div class="classes-list">
    <!-- Header Section -->
    <section class="classes-header text-white text-center py-5 mb-4">
      <div class="container">
        <h1 class="display-4 fw-bold mb-3">Our Lessons</h1>
        <p class="lead">
          Explore concise, hands-on lessons to quickly build practical computer science skills.
        </p>
      </div>
    </section>

    <!-- Sort Controls -->
    <div class="row mb-4">
      <div class="col-md-6">
        <label class="form-label">Sort By:</label>
        <select 
          class="form-select" 
          :value="sortAttribute"
          @change="$emit('update:sortAttribute', $event.target.value)"
        >
          <option value="subject">Subject</option>
          <option value="location">Location</option>
          <option value="price">Price</option>
          <option value="space">Availability</option>
        </select>
      </div>
      <div class="col-md-6">
        <label class="form-label">Order:</label>
        <select 
          class="form-select"
          :value="sortOrder"
          @change="$emit('update:sortOrder', $event.target.value)"
        >
          <option value="asc">Ascending</option>
          <option value="desc">Descending</option>
        </select>
      </div>
    </div>

    <!-- Lessons Grid -->
    <div class="row">
      <div 
        class="col-md-4 mb-4" 
        v-for="lesson in lessons" 
        :key="lesson._id"
      >
        <div class="card h-100 shadow-sm lesson-card">
          <div class="card-body">
            <div class="text-center mb-3">
              <i :class="'fas ' + lesson.icon + ' fa-3x text-primary'"></i>
            </div>
            <h5 class="card-title">{{ lesson.subject }}</h5>
            <ul class="list-unstyled">
              <li>
                <i class="fas fa-map-marker-alt text-danger"></i> 
                <strong>Location:</strong> {{ lesson.location }}
              </li>
              <li>
                <i class="fas fa-pound-sign text-success"></i> 
                <strong>Price:</strong> £{{ lesson.price }}
              </li>
              <li>
                <i class="fas fa-users text-info"></i> 
                <strong>Spaces:</strong> {{ lesson.space }}
              </li>
            </ul>
          </div>
          <div class="card-footer bg-transparent">
            <button 
              class="btn btn-primary w-100" 
              @click="$emit('add-to-cart', lesson)"
              :disabled="lesson.space === 0"
            >
              <i class="fas fa-cart-plus"></i> 
              {{ lesson.space > 0 ? 'Add to Cart' : 'No Spaces Available' }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- No Results Message -->
    <div v-if="lessons.length === 0" class="alert alert-info text-center">
      <i class="fas fa-search"></i> 
      {{ searchQuery ? 'No lessons found matching your search.' : 'No lessons available at the moment.' }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'ClassesList',
  props: {
    lessons: {
      type: Array,
      required: true
    },
    sortAttribute: {
      type: String,
      required: true
    },
    sortOrder: {
      type: String,
      required: true
    },
    searchQuery: {
      type: String,
      default: ''
    }
  },
  emits: ['update:sortAttribute', 'update:sortOrder', 'add-to-cart']
}
</script>

<style scoped>
.classes-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 10px;
}

.lesson-card {
  transition: transform 0.2s, box-shadow 0.2s;
  border: none;
}

.lesson-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.card-title {
  color: #0d6efd;
  font-weight: bold;
  font-size: 1.3rem;
}

.card-body ul li {
  margin-bottom: 0.5rem;
}

.btn-primary:disabled {
  cursor: not-allowed;
}

.fa-map-marker-alt,
.fa-pound-sign,
.fa-users {
  margin-right: 0.5rem;
}

.card-footer {
  border-top: 1px solid rgba(0, 0, 0, 0.125);
}

.form-control:focus {
  border-color: #0d6efd;
  box-shadow: 0 0 0 0.2rem rgba(13, 110, 253, 0.25);
}

h2 {
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
</style>
