<template>
    <!-- Template pour la création d'un utilisateur -->
    <template v-if="usage === 'create'">
        <div class="container">
            <label for="createfirstname">Prénom</label>
            <input v-model="newUser.firstName" type="text" class="form-control"
                   placeholder="Insérez votre prénom">
            <label for="createlastname">Nom</label>
            <input v-model="newUser.lastName" type="text" class="form-control"
                   placeholder="Insérez votre nom">
            <label for="emaiC">Email</label>
            <input v-model="newUser.email" type="email" class="form-control"
                   placeholder="Insérez votre email">
            <label for="createbirthdate">Date de naissance</label>
            <input v-model="newUser.birthDate" type="date" class="form-control">
            <label for="avatarUrl">Photo</label>
            <input id="avatarUrl" class="form-control" type="file" @change="onFileChange"/>
            <label for="creategender">Genre</label>
            <select class="form-select" v-model="newUser.gender">
                <option value="male">Homme</option>
                <option value="female">Femme</option>
            </select>
            <button type="submit" class="btn btn-success" @click="createUser">Créer</button>
        </div>
    </template>

    <!-- Template pour la modification d'un utilisateur -->
    <template v-if="usage === 'edit'">
        <div class="container">
            <div class="img-edit">
                <img :src="`http://localhost:8081${updateUser.avatarUrl}`">
            </div>
            <label for="firstname">Prénom</label>
            <input v-model="updateUser.firstName" type="text" class="form-control" id="firstname"
                   placeholder="Insérez votre prénom">
            <label for="lastname">Nom</label>
            <input v-model="updateUser.lastName" type="text" class="form-control" id="lastname"
                   placeholder="Insérez votre nom">
            <label for="email">Email</label>
            <input v-model="updateUser.email" type="email" class="form-control" id="email"
                   placeholder="Insérez votre email">
            <label for="birthdate">Date de naissance</label>
            <input v-model="updateUser.birthDate" type="date" class="form-control" id="birthdate">
            <label for="avatarUrl">Photo</label>
            <input id="avatarUrl" class="form-control" type="file" @change="onFileChange"/>
            <label for="gender">Genre</label>
            <select class="form-select" id="gender" v-model="updateUser.gender">
                <option value="male">Homme</option>
                <option value="female">Femmme</option>
            </select>
            <button type="submit" class="btn btn-success" @click="editUser">Editer</button>
        </div>
    </template>
</template>

<script>
import axios from "axios";

export default {
    name: "Form",
    props: {
        user: Object,
        usage: String
    },
    data() {
        return {
            newUser: {},
            updateUser: {},
        }
    },
    methods: {
        // Créer un utilisateur
        createUser() {
            const userData = new FormData();
            userData.append('firstName', this.newUser.firstName);
            userData.append('lastName', this.newUser.lastName);
            userData.append('email', this.newUser.email);
            userData.append('birthDate', this.newUser.birthDate);
            userData.append('gender', this.newUser.gender);

           axios
                .post('http://localhost:8081/users', userData)
                .then(() => {
                    this.$toast.success("utilisateur créé.", {
                        position: 'bottom-right'
                    });
                    setTimeout(() => {
                        this.$router.go(-1);
                    }, 2000);
                })
                .catch(() => {
                    this.$toast.error("ce mail existe déja", {position: 'bottom-right'});
                })
        },
        // Modifier un utilisateur
        editUser() {
            axios
                .put(`http://localhost:8081/users/${this.updateUser.id}`, this.updateUser)
                .then(() => {
                    this.$toast.success("utilisateur modifié", {
                        position: 'bottom-right'
                    });
                    setTimeout(() => {
                        this.$router.go(-1);
                    }, 3000);
                })
        }
    },
    watch: {
        user() {
            this.updateUser = this.user;
        }
    }
}
</script>

<style scoped>
.container {
    text-align: start;
}

input, select {
    margin-bottom: 20px;
}

.img-edit {
    margin: auto;
    width: 400px;
}

img {
    width: 100%;
}
</style>
