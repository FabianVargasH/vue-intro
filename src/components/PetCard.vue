<script setup>
import { computed } from 'vue'

const props = defineProps({
  pet: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits([
  'toggle-vaccinated',
  'delete-pet',
])

// Icono según la especie
const petIcon = computed(() => {
  const icons = {
    Perro: '🐶',
    Gato: '🐱',
    Ave: '🐦',
    Conejo: '🐰',
    Otro: '🐾',
  }
  return icons[props.pet.species] || '🐾'
})

// Función para cambiar el estado de vacunación
function toggleVaccinated() {
  emit('toggle-vaccinated', props.pet.id)
}

// Función para eliminar la mascota
function deletePet() {
  emit('delete-pet', props.pet.id)
}
</script>

<template>
  <article class="pet-card" :class="{ 'pet-card--vaccinated': pet.vaccinated }">
    <div class="pet-header">
      <span class="pet-icon">{{ petIcon }}</span>
      <h3 class="pet-name">{{ pet.name }}</h3>
      <span class="pet-species">{{ pet.species }}</span>
    </div>
    
    <div class="pet-info">
      <p><strong>Edad:</strong> {{ pet.age }} años</p>
      <p><strong>Propietario:</strong> {{ pet.owner }}</p>
      <p class="vaccination-status">
        <span v-if="pet.vaccinated" class="status-vaccinated">Vacunada</span>
        <span v-else class="status-not-vaccinated">No vacunada</span>
      </p>
    </div>
    
    <div class="pet-actions">
      <button @click="toggleVaccinated" class="btn-toggle">
        Cambiar estado
      </button>
      <button @click="deletePet" class="btn-delete">Eliminar</button>
    </div>
  </article>
</template>

<style scoped>
.pet-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  border: 2px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.pet-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
}

/* Diseño diferente según estado de vacunación */
.pet-card--vaccinated {
  border-color: #28a745;
  background: #f0fff4;
}

.pet-card:not(.pet-card--vaccinated) {
  border-color: #dc3545;
  background: #fff5f5;
}

.pet-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex-wrap: wrap;
}

.pet-icon {
  font-size: 2.5rem;
  line-height: 1;
}

.pet-name {
  margin: 0;
  font-size: 1.25rem;
  font-weight: 700;
  color: #2c3e50;
}

.pet-species {
  background: #e9ecef;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.875rem;
  color: #495057;
  margin-left: auto;
}

.pet-info {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.pet-info p {
  margin: 0;
  color: #333;
  font-size: 0.95rem;
}

.pet-info p strong {
  color: #2c3e50;
  font-weight: 600;
}

.vaccination-status {
  font-weight: 600;
  margin-top: 0.25rem;
}

.status-vaccinated {
  color: #28a745;
  font-size: 1rem;
}

.status-not-vaccinated {
  color: #dc3545;
  font-size: 1rem;
}

.pet-actions {
  display: flex;
  gap: 0.75rem;
  margin-top: 0.5rem;
  flex-wrap: wrap;
}

.btn-toggle {
  background: #42b883;
  color: white;
  border: none;
  padding: 0.6rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
  flex: 1;
  font-size: 0.9rem;
  font-weight: 500;
}

.btn-toggle:hover {
  background: #359268;
  transform: scale(1.02);
}

.btn-delete {
  background: #dc3545;
  color: white;
  border: none;
  padding: 0.6rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
  font-size: 0.9rem;
  font-weight: 500;
}

.btn-delete:hover {
  background: #c82333;
  transform: scale(1.02);
}

@media (max-width: 768px) {
  .pet-card {
    padding: 1rem;
  }
  
  .pet-header {
    flex-wrap: wrap;
  }
  
  .pet-species {
    margin-left: 0;
    width: 100%;
    text-align: center;
  }
  
  .pet-actions {
    flex-direction: column;
  }
  
  .btn-toggle,
  .btn-delete {
    width: 100%;
    text-align: center;
  }
}

@media (max-width: 480px) {
  .pet-icon {
    font-size: 2rem;
  }
  
  .pet-name {
    font-size: 1.1rem;
  }
}
</style>