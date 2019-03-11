<template>
  <div>
    <h1>Home Page</h1>
    <p>Create User</p>
    <form @submit.prevent="onSubmit">
      <label for="name">name &nbsp;</label>
      <input type="text" v-model="name">
      <button type="sumbmit">submit</button>
    </form>

    <p>User list</p>
    <ul>
      <li v-for="user in users" :key="user.id">{{ user.id }} {{ user.name }}</li>
    </ul>
  </div>
</template>

<script>
import { gql } from "apollo-boost";
export default {
  data: () => ({
    name: ""
  }),
  methods: {
    async onSubmit() {
      if (this.name === "") return;

      const result = await this.$apollo.mutate({
        // Query
        mutation: gql`
          mutation($data: UserCreateInput!) {
            createUser(data: $data) {
              id
              name
            }
          }
        `,
        // Parameters
        variables: {
          data: { name: this.name }
        }
      });

      this.name = "";
      console.log(result);
    }
  },
  apollo: {
    users: gql`
      query {
        users {
          id
          name
        }
      }
    `
  }
};
</script>
