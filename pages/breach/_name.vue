<template>
  <div>
    <div class="hero is-info">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">Breach</h1>
          <h2 class="subtitle">{{$route.params.name}}</h2>

          <p v-html="breach.Description" />

          <form @submit.prevent="handleSearch(form.search, breach.Domain)">
            <b-field>
              <b-input v-model="form.search" type="text" />
              <button type="submit" class="button is-success">Search Account</button>
            </b-field>
          </form>

          <p v-if="breached === false">Not Found!</p>
          <p v-else-if="breached === true" class="has-text-warning has-text-weight-bold is-size-2">Found! {{breached}}</p>
        </div>
      </div>
    </div>

    <div class="section">
      <div class="container">
        <h2 class="subtitle">Other places your account was found</h2>

        <b-table
          :data="pastes"
        >
          <template slot-scope="props">
            <b-table-column label="Source">
              {{props.row.Source}}
            </b-table-column>
            <b-table-column label="Title">
              {{props.row.Title}}
            </b-table-column>
            <b-table-column label="Link">
              {{props.row.Id}}
            </b-table-column>
          </template>
        </b-table>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import axios from 'axios'
  import BTable from "buefy/src/components/table/Table";

  export default {
    components: {BTable},
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