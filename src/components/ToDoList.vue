<script setup>
import { ref, defineProps, defineEmits } from "vue";

const props = defineProps(["medicament"]);
const emit = defineEmits([
  "deleteMedicament",
  "updateMedicament",
  "increment",
  "decrement",
]);

const isEditing = ref(false);
const nouveauNom = ref(props.medicament.denomination);
const nouveauForme = ref(props.medicament.formepharmaceutique);
const nouveauPhoto = ref(null);
const hasnouveauPhoto = ref(false);

const Upload = (event) => {
  const file = event.target.files[0];
  if (file) {
    const validTypes = ["image/jpeg", "image/png", "image/jpg"];
    if (!validTypes.includes(file.type)) {
      return;
    }
    const reader = new FileReader();
    reader.onload = () => {
      nouveauPhoto.value = reader.result.split(",")[1];
      hasnouveauPhoto.value = true;
    };
    reader.readAsDataURL(file);
  }
};

const Changement = () => {
  const updatedMedicament = {
    id: props.medicament.id,
    denomination: nouveauNom.value,
    formepharmaceutique: nouveauForme.value,
    photo: hasnouveauPhoto.value ? nouveauPhoto.value : null,
  };
  emit("updateMedicament", updatedMedicament);
  isEditing.value = false;
  hasnouveauPhoto.value = false;
};
</script>

<template>
  <li class="list-item">
    <div v-if="!isEditing" class="medicament-info">
      <span class="medicament-text">
        {{ medicament.denomination }}<br />
        {{ medicament.formepharmaceutique }}<br />
        {{ medicament.qte }}
      </span>
      <div class="button-group">
        <button @click="isEditing = true" class="edit-button">Modifier</button>
        <button
          @click="$emit('deleteMedicament', medicament.id)"
          class="delete-button"
        >
          Supprimer
        </button>
      </div>

      <div class="quantity-buttons">
        <button
          @click="$emit('decrement', medicament.id)"
          class="action-button decrement-button"
        >
          -1
        </button>
        <button
          @click="$emit('increment', medicament.id)"
          class="action-button increment-button"
        >
          +1
        </button>
      </div>
    </div>
    <div v-else class="edit-form">
      <input
        type="text"
        v-model="nouveauNom"
        placeholder="Nom du médicament"
        class="input-field"
      />
      <input
        type="text"
        v-model="nouveauForme"
        placeholder="Forme pharmaceutique"
        class="input-field"
      />
      <input
        type="file"
        @change="Upload"
        accept="image/*"
        class="input-field"
      />
      <button @click="Changement" class="save-button">Valider</button>
      <button @click="isEditing = false" class="cancel-button">Annuler</button>
    </div>
    <div v-if="medicament.photo">
      <img
        :src="'https://apipharmacie.pecatte.fr/images/' + medicament.photo"
        alt="Photo du médicament"
        class="medicament-photo"
      />
    </div>
  </li>
</template>

<style scoped>
.list-item {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin: 5px 0;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: auto;
}

.medicament-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  gap: 10px;
}

.medicament-text {
  color: rgb(15, 15, 15);
  flex: 1;
  width: 100%;
  text-align: left;
  white-space: pre-line;
}

.button-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.quantity-buttons {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.edit-button,
.save-button,
.cancel-button,
.action-button,
.delete-button {
  padding: 8px 15px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.edit-button:hover,
.save-button:hover,
.cancel-button:hover,
.action-button:hover,
.delete-button:hover {
  background-color: #218838;
}

.increment-button,
.decrement-button {
  background-color: #28a745;
}

.delete-button {
  background-color: #dc3545;
}

.input-field {
  margin: 0 5px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 12px;
}

.input-field[type="file"] {
  color: black !important;
}

.input-field[type="file"]::file-selector-button {
  color: black !important;
  background-color: white;
  border: 1px solid #ccc;
  padding: 5px;
  border-radius: 4px;
  cursor: pointer;
}

.input-field[type="file"]::file-selector-button:hover {
  background-color: #f0f0f0;
}

.medicament-photo {
  max-width: 100px;
  max-height: 100px;
  object-fit: cover;
  border-radius: 4px;
}
</style>
