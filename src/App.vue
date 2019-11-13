<template>
  <div id="app">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.css">

    <div id="users">
      <button @click="searchUsers" id="fetch-users" class="btn btn-primary">Fetch users</button>
      <label>
        <input class="form-control align-middle d-inline-block" type="text" v-model="filter.name" placeholder="Name Search"
               style="width: auto;">
      </label>
      <div class="input-group align-middle" style="display:inline-block; width: auto;">
        <input id="gender-male" class="d-none" type="radio" v-model="filter.gender" value="male">
        <label for="gender-male"><i class="fas fa-mars" style="font-size: 30px"></i></label>
        <input id="gender-female" class="d-none" type="radio" v-model="filter.gender" value="female">
        <label for="gender-female"><i class="fas fa-venus" style="font-size: 30px"></i></label>
        <input id="gender-*" class="d-none" type="radio" v-model="filter.gender" value="*">
        <label for="gender-*"><i class="fas fa-transgender" style="font-size: 30px"></i></label>
      </div>
      <div class="mt-2">Result: {{ usersSorted.length }} / {{ allUsers.length }}</div>

<!--      <User @click="" v-for="(u, index) in usersSorted" :key="index"-->
<!--                 :value="u"></User>-->
<!--      <Users></Users>-->
      <div class="Users"  >
        <table id="tbl-users" class="table table-hover">
          <thead>
          <tr>
            <th></th>
            <th @click="changeSort('name')">Nom <i class="fas" :class="getNameIcon"></i></th>
            <th @click="changeSort('email')">Email <i class="fas" :class="getEmailIcon"></i></th>
            <th>Tel</th>
            <th @click="changeSort('age')">Age <i class="fas" :class="getAgeIcon"></i></th>
          </tr>
          </thead>
          <tbody>
          <Users @click="showUserModal" v-for="(u, index) in usersSorted" :key="index"
                     :value="u"></Users>
<!--          <tr v-for="u in usersSorted" :key="u">-->
<!--            <td><img :src='u.picture.thumbnail'/></td>-->
<!--            <td>{{u.name.first}} {{u.name.last}}</td>-->
<!--            <td>{{u.email}}</td>-->
<!--            &lt;!&ndash;            <td>{{u.id.value}}</td>&ndash;&gt;-->
<!--            <td>{{u.phone}}</td>-->
<!--            <td>{{u.dob.age}}</td>-->
<!--          </tr>-->
          </tbody>
        </table>
      </div>

    </div>

    <UserModal v-show="isModalVisible" @close="closeUserModal">

    </UserModal>
  </div>
</template>

<script>
import UserModal from './components/UserModal.vue';
import axios from "axios";
import Users from "./components/Users";

export default {
  components: {
    Users,
    UserModal,
  },
data(){
    return{
            isModalVisible: false,
            allUsers: [],
            sort: {
              name: 0,
              email: 0,
              age: 0,
            },
            filter: {
              name: '',
              gender: '*',
            },
            icons: ['fa-sort', 'fa-sort-up', 'fa-sort-down']
          }
  },
  computed: {
    usersSorted() {
      return this.allUsers
              .slice(0)
              .filter(u => {
                return this.filter.name === ''
                        || `${u.name.first} ${u.name.last}`.toLowerCase().includes(this.filter.name.toLowerCase());
              })
              .filter(u => {
                return this.filter.gender === '*' || this.filter.gender === u.gender;
              })
              .sort(((a, b) => {
                let compare = 0;

                const aName = `${a.name.first} ${a.name.last}`;
                const bName = `${b.name.first} ${b.name.last}`;

                if (this.sort.name === 1 && compare === 0) {
                  compare = aName.localeCompare(bName);
                } else if (this.sort.name === 2 && compare === 0) {
                  compare = -aName.localeCompare(bName);
                }

                if (this.sort.email === 1 && compare === 0) {
                  compare = a.email.localeCompare(b.email);
                } else if (this.sort.email === 2 && compare === 0) {
                  compare = -a.email.localeCompare(b.email);
                }

                if (this.sort.age === 1 && compare === 0) {
                  if (a.dob.age > b.dob.age) compare = 1;
                  else if (a.dob.age < b.dob.age) compare = -1;
                } else if (this.sort.age === 2 && compare === 0) {
                  if (a.dob.age > b.dob.age) compare = -1;
                  else if (a.dob.age < b.dob.age) compare = 1;
                }

                return compare;
              }));
    },
    getNameIcon() {
      return this.icons[this.sort.name];
    },
    getEmailIcon() {
      return this.icons[this.sort.email];
    },
    getAgeIcon() {
      return this.icons[this.sort.age];
    }
  },
  methods: {
    async searchUsers() {
      const {data: {results}} = await axios.get('https://randomuser.me/api/?results=150');

      this.allUsers = results;
    },
    changeSort(type) {
      this.sort[type]++;
      if (this.sort[type] > 2) this.sort[type] = 0;
    },
    showUserModal(){
      this.isModalVisible = true;
      // this.loadUserDetails(id);
    },
    closeUserModal(){
      this.isModalVisible = false;
    },
    // loadUserDetails(id){
    //   alert(id);
    // },
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}


</style>
