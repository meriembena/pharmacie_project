<script setup>
import { ref } from "vue";

const nom = ref("");
const forme = ref("");
const quantiteStock = ref("");
const image = ref(null);
const emit = defineEmits(["ajouter"]);

const ajouter = () => {
  emit("ajouter", nom.value, forme.value, quantiteStock.value, image.value);
};

const UploadFichier = (event) => {
  const fichier = event.target.files[0];
  if (!fichier) return;
  const lecteur = new FileReader();
  lecteur.onload = () => {
    image.value = lecteur.result;
  };
  lecteur.readAsDataURL(fichier);
};

const reinitialiserFormulaire = () => {
  nom.value = "";
  forme.value = "";
  quantiteStock.value = "";
  image.value = null;
};
</script>

<template>
  <div class="formulaire-conteneur">
    <form @submit.prevent="ajouter" class="formulaire-style">
      <div class="form-row">
        <input
          type="text"
          v-model="nom"
          placeholder="Dénomination"
          required
          class="champ-style"
        />
        <input
          type="text"
          v-model="forme"
          placeholder="Forme"
          required
          class="champ-style"
        />
        <input
          type="number"
          v-model="quantiteStock"
          placeholder="Quantité"
          required
          class="champ-style"
        />
      </div>
      <div class="form-row">
        <input
          type="file"
          @change="UploadFichier"
          accept="image/*"
          class="champ-style fichier-input"
        />
      </div>
      <div class="boutons-container">
        <button type="submit" class="bouton-ajouter">Ajouter</button>
        <button
          type="button"
          class="bouton-annuler"
          @click="reinitialiserFormulaire"
        >
          Annuler
        </button>
      </div>
    </form>
  </div>
</template>

<style scoped>
.formulaire-conteneur {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #e9f7e4;
  font-family: "Arial", sans-serif;
}

.formulaire-style {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 600px;
  padding: 20px;
  background-color: transparent;
  box-shadow: none;
  border-radius: 0;
  margin: 20px;
}

.form-row {
  display: flex;
  justify-content: space-between;
  margin: 8px 0;
}

.form-row input {
  flex: 1;
  margin-right: 10px;
}

.form-row input:last-child {
  margin-right: 0;
}

.champ-style {
  padding: 8px;
  font-size: 12px;
  border: none;
  border-bottom: 2px solid #28a745;
  outline: none;
  background-color: transparent;
  transition: border-color 0.3s ease;
  width: 150px;
}

.champ-style:focus {
  border-bottom: 2px solid #218838;
}

.fichier-input {
  padding: 6px;
  background-color: transparent;
  border-bottom: 2px solid #28a745;
}

.boutons-container {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

.bouton-ajouter {
  padding: 8px 16px;
  background-color: #28a745;
  color: white;
  font-size: 14px;
  font-weight: bold;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  width: 120px;
  text-align: center;
}

.bouton-ajouter:hover {
  background-color: #218838;
}

.bouton-annuler {
  padding: 8px 16px;
  background-color: #dc3545;
  color: white;
  font-size: 14px;
  font-weight: bold;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  width: 120px;
  text-align: center;
}

.bouton-annuler:hover {
  background-color: #c82333;
}
</style>
