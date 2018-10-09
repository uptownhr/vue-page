<template>
  <div>
    <h2>Home Page</h2>

    <form @submit.prevent="handleSearch(form.search)">
      <input v-model="form.search" type="text" />
      <button type="submit">Search</button>
    </form>

    <table>
      <thead>
      <tr>
        <th>Title</th>
        <th>Domain</th>
        <th>Breached</th>
        <th>Description</th>
        <th>Breach Date</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="breach in breaches">
        <td>
          <nuxt-link :to="`/breach/${breach.Name}`">{{breach.Title}}</nuxt-link>
        </td>
        <td>{{breach.Domain}}</td>
        <td><pre>{{breach.DataClasses}}</pre></td>
        <td v-html="breach.Description"></td>
        <td>{{breach.BreachDate}}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script type="text/ecmascript-6">
  import axios from 'axios'

  export default {
    data () {
      return {
        form: {
          search: ''
        },
        breaches: []
      }
    },

    methods: {
      handleSearch (searchText) {
        return axios.get(`https://haveibeenpwned.com/api/v2/breaches?domain=${searchText}`)
          .then(res => {
            this.breaches = res.data
          })
      }
    }
  }
</script>