<template>
  <q-page>
    <q-card>
      <q-card-section>
        <h2>Customer Testimonials</h2>
      </q-card-section>
      <q-card-section>
        <div v-if="testimonials.length">
          <div class="row q-col-gutter-md">
            <div v-for="testimonial in testimonials" :key="testimonial.id" class="col-12 col-sm-6 col-md-4">
              <q-card :style="get(testimonial.id)" class="testimonial-card">
                <q-card-section>
                  
                  <h3>{{ testimonial.attributes.name }} - {{ testimonial.attributes.position }}</h3>
                  <p>{{ testimonial.attributes.testimonial_text }}</p>
                  <p><strong>Company:</strong> {{ testimonial.attributes.company }}</p>
                  <p><strong>Rating:</strong> {{ testimonial.attributes.rating }} / 5</p>
                  <p><strong>Featured:</strong> {{ testimonial.attributes.featured ? 'Yes' : 'No' }}</p>
                  <p><small>Published on: {{ new Date(testimonial.attributes.publishedAt).toLocaleDateString() }}</small></p>
                </q-card-section>
              </q-card>
            </div>
          </div>
        </div>
        <div v-else>
          <p>No Content Available</p>
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

// Define Testimonial Type
interface Testimonial {
  id: number;
  attributes: {
    name: string;
    position: string;
    company: string;
    testimonial_text: string;
    rating: number;
    featured: boolean;
    createdAt: string;
    updatedAt: string;
    publishedAt: string;
  };
}

const testimonials = ref<Testimonial[]>([]);

async function fetchTestimonials(){
  try {
    const response = await axios.get('https://www.marketing-backend.scientiflow.com/api/testimonials', {
      headers: {
        Authorization: 'Bearer 9d13375ffa60153ff36a8b802ff4f4e72aec52e0a5ad752a435e3d26736d36a80479056f21d5831f6a5a783f2524987ae51be2c88c1032f642cfb110d7577e567a49e65f83e7cb0aca79037ee8fb295ecc160cb5cb4b8d317582379bb10403a788b090ed2e3703da52b1f84cd2ff824e387990269cbbedbc614ef1b969d63666', // Replace with your actual bearer token
      },
    });

    testimonials.value = response.data.data; 
  } catch (error) {
    console.error('Error fetching testimonials:', error);
  }
};


onMounted(fetchTestimonials);

function get(id: number) {
  const lightColors = ['#FFEBEE', '#FFF9C4', '#E0F7FA', '#FFF3E0', '#E8F5E9'];
  const backgroundColor = lightColors[id % lightColors.length]; 
  return {
    backgroundColor,
    padding: '2rem',
    borderRadius: '8px',
    minHeight: '200px',
  };
}

</script>

<style scoped>
.testimonial-card {
  border-radius: 8px;
  padding: 12px;
  margin-bottom: 16px;
  margin-left: 0.2rem;
  transform: scale(0.9); 
}

h2{
  text-align: center;
}
</style>
