<template>
  <div class="card mb-3">
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
          <button class="btn btn-danger btn-sm" @click="$emit('remove')">
            <i class="fas fa-trash"></i> Remove
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CartItem',
  props: {
    item: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    }
  },
  emits: ['remove']
}
</script>

<style scoped>
.card {
  transition: transform 0.2s;
}

.card:hover {
  transform: translateX(5px);
}
</style>
