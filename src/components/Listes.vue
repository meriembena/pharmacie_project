<script setup>
import { ref, reactive, onMounted, computed } from "vue";
import Medicament from "../Medicament";
import Formulaire from "./Formulaire.vue";
import ToDoList from "./ToDoList.vue";
const medicaments = reactive([]);
const searchQuery = ref("");
const apiUrl = "https://apipharmacie.pecatte.fr/api/5/medicaments";
const isFormVisible = ref(false);

const fetchMedicaments = () => {
  fetch(apiUrl)
    .then((response) => response.json())
    .then((data) => {
      medicaments.splice(0, medicaments.length);
      data.forEach((item) =>
        medicaments.push(
          new Medicament(
            item.id,
            item.denomination,
            item.formepharmaceutique,
            item.qte,
            item.photo
          )
        )
      );
    });
};
const updateMedicament = (updatedMedicament) => {
  fetch(`${apiUrl}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(updatedMedicament),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data.status === 1) {
        fetchMedicaments();
      } else {
        console.error(
          "il y a un erreur pour mettre à jour du médicament:",
          data
        );
      }
    })
    .catch((error) =>
      console.error("il y a un erreur pour mettre à jour du médicament:", error)
    );
};
onMounted(() => {
  fetchMedicaments();
});

const filteredMedicaments = computed(() => {
  return medicaments.filter((medicament) =>
    medicament.denomination
      .toLowerCase()
      .includes(searchQuery.value.toLowerCase())
  );
});
const updateMedicamentInAPI = (medicament) => {
  fetch(apiUrl, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      id: medicament.id,
      denomination: medicament.denomination,
      formepharmaceutique: medicament.formepharmaceutique,
      qte: medicament.qte,
      photo: medicament.photo,
    }),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data.status === 1) {
        const index = medicaments.findIndex((m) => m.id === medicament.id);
        if (index !== -1) {
          medicaments[index] = medicament;
        }
      } else {
        console.error("Erreur lors de la mise à jour:", data);
      }
    })
    .catch((error) =>
      console.error("il y a un erreur pour mettre à jour du médicament:", error)
    );
};

const addMedicament = (name, form, quantity, photo = "") => {
  fetch(apiUrl, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      denomination: name,
      formepharmaceutique: form,
      qte: quantity,
      photo: photo,
    }),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data.status === 1) {
        fetchMedicaments();
      } else {
        console.error("il y a un erreur pour ajouter du médicament:", data);
      }
    })
    .catch((error) =>
      console.error("il y a un erreur pour ajouter du médicament:", error)
    );
};

const deleteMedicament = (id) => {
  fetch(`${apiUrl}/${id}`, { method: "DELETE" })
    .then((response) => response.json())
    .then((data) => {
      if (data.status === 1) {
        fetchMedicaments();
      }
    })
    .catch((error) =>
      console.error("il y a un erreur pour supprimer du médicament:", error)
    );
};

const increment = (id) => {
  const medicament = medicaments.find((m) => m.id === id);
  if (medicament) {
    medicament.qte += 1;
    updateMedicamentInAPI(medicament);
  }
};

const decrement = (id) => {
  const medicament = medicaments.find((m) => m.id === id);
  if (medicament && medicament.qte > 0) {
    medicament.qte -= 1;
    updateMedicamentInAPI(medicament);
  }
};

const toggleFormVisibility = () => {
  isFormVisible.value = !isFormVisible.value;
  console.log(isFormVisible.value);
};
</script>
<template>
  <div class="container">
    <div class="search-container">
      <input
        type="text"
        v-model="searchQuery"
        placeholder="Tapez pour trouver un médicament"
        class="search-input"
      />
    </div>

    <ul class="medicament-list">
      <ToDoList
        v-for="medicament in filteredMedicaments"
        :key="medicament.id"
        :medicament="medicament"
        @deleteMedicament="deleteMedicament"
        @increment="increment"
        @decrement="decrement"
        @updateMedicament="updateMedicament"
      />
    </ul>

    <button @click="toggleFormVisibility" class="btn-ajouter">
      Ajouter un médicament
    </button>

    <Formulaire v-if="isFormVisible" @ajouter="addMedicament" />
  </div>
</template>

<style scoped>
.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 40px 20px;
  background-color: #ffffff;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  font-family: "Roboto", sans-serif;
}

.search-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.search-input {
  width: 60%;
  padding: 12px 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  background-color: #f5f5f5;
  color: #333;
}

.search-input:focus {
  border-color: #007bff;
  background-color: #ffffff;
}

.medicament-list {
  list-style-type: none;
  padding: 0;
}

.medicament-list li {
  display: flex;
  justify-content: space-between;
  padding: 15px;
  margin-bottom: 10px;
  background-color: #f9f9f9;
  border-radius: 10px;
}

.btn-ajouter {
  padding: 15px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
}

.btn-ajouter:hover {
  background-color: #218838;
}
</style>
