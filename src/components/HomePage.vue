<template>
  <div class="home-page">
    <!-- Hero Section -->
    <section class="hero-section text-white">
      <div class="container">
        <div class="row">
          <div class="col-lg-6">
            <h1 class="display-3 fw-bold mb-4">Welcome!</h1>
            <p class="mb-4">
              Discover a variety of engaging after-school lessons designed to help you master new skills 
              and deepen your understanding of computer science. Each lesson offers clear explanations, 
              practical examples, and hands-on activities to support learners at every level.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- Features Section -->
    <section class="features-section py-5">
      <div class="container">
        <h2 class="text-center mb-5">Why Choose MINN EDU?</h2>
        <div class="row">
          <div class="col-md-4 mb-4">
            <div class="card h-100 text-center feature-card">
              <div class="card-body">
                <i class="fas fa-chalkboard-teacher fa-3x text-black mb-3"></i>
                <h5 class="card-title">Expert Instructors</h5>
                <p class="card-text">
                  Learn from experienced professionals who are passionate about teaching and technology.
                </p>
              </div>
            </div>
          </div>
          <div class="col-md-4 mb-4">
            <div class="card h-100 text-center feature-card">
              <div class="card-body">
                <i class="fas fa-laptop-code fa-3x text-black mb-3"></i>
                <h5 class="card-title">Hands-On Learning</h5>
                <p class="card-text">
                  Practice with real-world projects and gain practical skills you can use immediately.
                </p>
              </div>
            </div>
          </div>
          <div class="col-md-4 mb-4">
            <div class="card h-100 text-center feature-card">
              <div class="card-body">
                <i class="fas fa-clock fa-3x text-black mb-3"></i>
                <h5 class="card-title">Flexible Schedule</h5>
                <p class="card-text">
                  Choose from various time slots that fit your busy after-school schedule.
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="cta-section text-center py-5 text-white">
      <div class="container">
        <h2 class="mb-4">Ready to Start Learning?</h2>
        <p class="lead mb-4">Join thousands of students already improving their computer science skills!</p>
        <button class="btn btn-light btn-lg" @click="$emit('navigate-to-classes')">
          <i class="fas fa-arrow-right"></i> View All Lessons
        </button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  emits: ['navigate-to-classes']
}
</script>

<style scoped>
.hero-section {
  background-image: url('@/assets/lessons-hero.jpg');
  background-size: cover;
  background-position: center;
  padding: 100px 20px;
  min-height: 200px;
  position: relative;
}

.hero-section > .container {
  position: relative;
}

.features-section {
  background-color: #f8f9fa;
}

.feature-card {
  border: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s, box-shadow 0.3s;
}

.feature-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
}

.cta-section {
  background-color: black;
}

h2 {
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
</style>
