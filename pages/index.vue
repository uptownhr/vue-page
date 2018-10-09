<template>
  <div>
    <div class="hero is-primary">
      <div class="section">
        <div class="container">
          <div class="hero-body">
            <h1 class="title">Home Page</h1>
            <h2 class="subtitle">Hacked domain?</h2>
            <form @submit.prevent="handleSearch(form.search)">
              <b-field>
                <b-input v-model="form.search" type="text" placeholder="tumblr.com" />
                <button type="submit" class="button is-info">Search</button>
              </b-field>
            </form>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="section">
        <h2 class="title">Results</h2>

        <b-table
          :data="breaches"
        >
          <template slot-scope="props">
            <b-table-column label="Title">
              <nuxt-link :to="`/breach/${props.row.Name}`">{{props.row.Title}}</nuxt-link>
            </b-table-column>
            <b-table-column label="Domain">
              {{props.row.Domain}}
            </b-table-column>
            <b-table-column label="Breached">
              {{props.row.DataClasses.join(',')}}
            </b-table-column>
            <b-table-column label="Description">
              <p v-html="props.row.Description"></p>
            </b-table-column>
            <b-table-column label="Date">
              {{props.row.BreachDate}}
            </b-table-column>
          </template>
        </b-table>
      </div>
    </div>
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