<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import PetCard from './components/PetCard.vue'

const pets = ref([
  {
    id: 1,
    name: 'Luna',
    species: 'Perro',
    age: 4,
    owner: 'Andrea Vargas',
    vaccinated: true,
  },
  {
    id: 2,
    name: 'Milo',
    species: 'Gato',
    age: 2,
    owner: 'Carlos Méndez',
    vaccinated: false,
  },
  {
    id: 3,
    name: 'Kiwi',
    species: 'Ave',
    age: 1,
    owner: 'Sofía Ramírez',
    vaccinated: true,
  },
])

// Variables para el formulario
const newName = ref('')
const newSpecies = ref('Perro')
const newAge = ref(1)
const newOwner = ref('')
const newVaccinated = ref(false)

// Lista de especies disponibles
const speciesOptions = ['Perro', 'Gato', 'Ave', 'Conejo', 'Otro']

// Variables para el buscador y filtro
const searchText = ref('')
const speciesFilter = ref('Todas')

// Valor calculado para filtrar mascotas
const filteredPets = computed(() => {
  const text = searchText.value.trim().toLowerCase()
  return pets.value.filter((pet) => {
    const matchesText =
      pet.name.toLowerCase().includes(text) ||
      pet.owner.toLowerCase().includes(text)
    const matchesSpecies =
      speciesFilter.value === 'Todas' ||
      pet.species === speciesFilter.value
    return matchesText && matchesSpecies
  })
})

// Función para agregar mascota con validaciones
function addPet() {
  // 1. Verificar que el nombre no esté vacío
  if (!newName.value.trim()) {
    alert('El nombre de la mascota es obligatorio')
    return
  }
  
  // 2. Verificar que se haya seleccionado una especie
  if (!newSpecies.value) {
    alert('Debes seleccionar una especie')
    return
  }
  
  // 3. Verificar que el propietario no esté vacío
  if (!newOwner.value.trim()) {
    alert('El nombre del propietario es obligatorio')
    return
  }
  
  // 4. Agregar la nueva mascota al arreglo
  pets.value.push({
    id: Date.now(),
    name: newName.value.trim(),
    species: newSpecies.value,
    age: Number(newAge.value),
    owner: newOwner.value.trim(),
    vaccinated: newVaccinated.value,
  })
  
  // 5. Limpiar todos los campos
  newName.value = ''
  newSpecies.value = 'Perro'
  newAge.value = 1
  newOwner.value = ''
  newVaccinated.value = false
}

// Función para cambiar el estado de vacunación
function toggleVaccinated(petId) {
  const pet = pets.value.find((item) => item.id === petId)
  if (pet) {
    pet.vaccinated = !pet.vaccinated
  }
}

// Función para eliminar una mascota
function deletePet(petId) {
  pets.value = pets.value.filter((pet) => {
    return pet.id !== petId
  })
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <!-- Formulario para agregar mascotas -->
    <form @submit.prevent="addPet" class="pet-form">
      <div class="form-group">
        <label for="name">Nombre:</label>
        <input type="text" id="name" v-model="newName" placeholder="Nombre de la mascota">
      </div>
      
      <div class="form-group">
        <label for="species">Especie:</label>
        <select id="species" v-model="newSpecies">
          <option v-for="species in speciesOptions" :key="species" :value="species">
            {{ species }}
          </option>
        </select>
      </div>
      
      <div class="form-group">
        <label for="age">Edad:</label>
        <input type="number" id="age" v-model.number="newAge" min="1">
      </div>
      
      <div class="form-group">
        <label for="owner">Propietario:</label>
        <input type="text" id="owner" v-model="newOwner" placeholder="Nombre del propietario">
      </div>
      
      <div class="form-group checkbox">
        <label for="vaccinated">¿Está vacunada?</label>
        <input type="checkbox" id="vaccinated" v-model="newVaccinated">
      </div>
      
      <button type="submit" class="btn-submit">Registrar</button>
    </form>

    <!-- Buscador y filtro -->
    <div class="search-filter">
      <div class="search-box">
        <label for="search">Buscar:</label>
        <input type="text" id="search" v-model="searchText" placeholder="Buscar por nombre...">
      </div>
      
      <div class="filter-box">
        <label for="filter">Filtrar por especie:</label>
        <select id="filter" v-model="speciesFilter">
          <option value="Todas">Todas</option>
          <option v-for="species in speciesOptions" :key="species" :value="species">
            {{ species }}
          </option>
        </select>
      </div>
    </div>

    <!-- Cuadrícula de mascotas -->
    <div class="results-info" v-if="filteredPets.length > 0">
      Mostrando {{ filteredPets.length }} de {{ pets.length }} mascotas
    </div>
    
    <section v-if="filteredPets.length > 0" class="pet-grid">
      <PetCard
        v-for="pet in filteredPets"
        :key="pet.id"
        :pet="pet"
        @toggle-vaccinated="toggleVaccinated"
        @delete-pet="deletePet"
      />
    </section>
    
    <section v-else class="empty-state">
      <p>No se encontraron mascotas.</p>
    </section>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

/* Estilos del formulario */
.pet-form {
  background: #f5f5f5;
  padding: 1.5rem;
  border-radius: 8px;
  margin-bottom: 1.5rem;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group.checkbox {
  flex-direction: row;
  align-items: center;
  gap: 0.5rem;
}

.form-group label {
  margin-bottom: 0.25rem;
  font-weight: 600;
  color: #2c3e50;
}

.form-group input,
.form-group select {
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.btn-submit {
  grid-column: span 2;
  background: #42b883;
  color: white;
  border: none;
  padding: 0.75rem;
  border-radius: 4px;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.3s;
}

.btn-submit:hover {
  background: #359268;
}

/* Buscador y filtro */
.search-filter {
  display: flex;
  gap: 2rem;
  margin: 1.5rem 0;
  padding: 1rem;
  background: #f5f5f5;
  border-radius: 8px;
  flex-wrap: wrap;
}

.search-box,
.filter-box {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex: 1;
}

.search-box input,
.filter-box select {
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  flex: 1;
}

.results-info {
  margin: 0.5rem 0 1rem;
  font-size: 0.9rem;
  color: #666;
}

/* Cuadrícula de mascotas */
.pet-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
  margin: 1rem 0;
}

/* Estado vacío */
.empty-state {
  text-align: center;
  padding: 3rem;
  color: #666;
  background: #f9f9f9;
  border-radius: 8px;
  margin: 1rem 0;
}

.empty-state p {
  font-size: 1.2rem;
  margin: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

@media (max-width: 768px) {
  .pet-form {
    grid-template-columns: 1fr;
  }
  
  .btn-submit {
    grid-column: span 1;
  }
}
</style>