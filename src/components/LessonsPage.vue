<template>
  <div>
    <h2 class="mb-4">Available Classes</h2>
    
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

    <!-- Lessons List -->
    <div class="row">
      <LessonCard
        v-for="lesson in sortedLessons"
        :key="lesson._id"
        :lesson="lesson"
        @add-to-cart="$emit('add-to-cart', lesson)"
      />
    </div>

    <!-- No Results Message -->
    <div v-if="sortedLessons.length === 0" class="alert alert-info">
      No lessons found matching your search.
    </div>
  </div>
</template>

<script>
import LessonCard from './LessonCard.vue'

export default {
  name: 'LessonsPage',
  components: {
    LessonCard
  },
  props: {
    sortedLessons: {
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
    }
  },
  emits: ['update:sortAttribute', 'update:sortOrder', 'add-to-cart']
}
</script>

<style scoped>
.form-control:focus {
  border-color: #0d6efd;
  box-shadow: 0 0 0 0.2rem rgba(13,110,253,0.25);
}
</style>
