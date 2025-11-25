<template>
  <div class="col-md-4 mb-4">
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
          @click="$emit('add-to-cart')"
          :disabled="lesson.space === 0"
        >
          <i class="fas fa-cart-plus"></i> 
          {{ lesson.space > 0 ? 'Add to Cart' : 'No Spaces Available' }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LessonCard',
  props: {
    lesson: {
      type: Object,
      required: true
    }
  },
  emits: ['add-to-cart']
}
</script>

<style scoped>
.lesson-card {
  transition: transform 0.2s, box-shadow 0.2s;
  border: none;
}

.lesson-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
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
  border-top: 1px solid rgba(0,0,0,0.125);
}
</style>
