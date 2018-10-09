<template>
  <div>
    <h1 class="title">Breach</h1>
    <h2 class="subtitle">{{$route.params.name}}</h2>

    <p v-html="breach.Description" />

    <form @submit.prevent="handleSearch(form.search, breach.Domain)">
      <input v-model="form.search" type="text" />
      <button type="submit">Search Account</button>
    </form>

    <pre v-if="breached === false">Not Found!</pre>
    <p v-else-if="breached === true">Found! {{breached}}</p>

    <h2>Other places your account was found</h2>
    <table border="1">
      <thead>
      <tr>
        <th>Source</th>
        <th>Title</th>
        <th>Link</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="paste in pastes">
        <td>{{paste.Source}}</td>
        <td>{{paste.Title}}</td>
        <td><a :href="paste.Id" target="_blank">{{paste.Id}}</a></td>
      </tr>
      </tbody>
    </table>

  </div>
</template>

<script type="text/ecmascript-6">
  import axios from 'axios'

  export default {
    async asyncData ({route}) {

      const breach = await axios.get(`https://haveibeenpwned.com/api/v2/breach/${route.params.name}`)
        .then(res => res.data)
        .catch(err => console.log(err))

      return {
        breach,
        form: {
          search: ''
        },
        breached: null,
        pastes: []
      }
    },

    methods: {
      async handleSearch (searchText, domain) {
        axios.get(`https://haveibeenpwned.com/api/v2/breachedaccount/${searchText}?domain=${domain}`)
          .then(res => {
            this.breached = true
          })
          .catch(err => {
            this.breached = false
          })

        axios.get(`https://haveibeenpwned.com/api/v2/pasteaccount/${searchText}`)
          .then(res => {
            this.pastes = res.data
          })
      }
    }
  }
</script>