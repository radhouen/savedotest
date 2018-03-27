<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <h2>Git Issues</h2>
    <div v-if="repos && repos.length">
      <ul v-for="repo of repos">

        <li class="list-group-item">{{repo.name}} , {{repo.git_url}} <span class="badge">{{repo.watchers}}</span></li>

      </ul>
    </div>

  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'app',
    data() {
      return {
        msg: 'Welcome to My Test Proposition',
        repos: [],
        per_page: 10,
        page: 1,
        pages: 5,
        total: 0,
        errors: [],
        load: false,
        time: new Date(),
      }
    },
    created() {
      this.getData()

    },
    watch: {},
    methods: {
      previous: function () {
        if (this.page > 1) {
          this.page = this.page - 1
          console.log(this.page)
          this.getData()
        }
      },

      next: function () {
        if (this.page < this.pages) {
          this.page = this.page + 1
          console.log(this.page)
          this.getData()
        }
      },

      getData: function () {
        this.load = true
        setTimeout(function () {
          this.load = true
        }, 10000)
        axios.get(`https://api.github.com/users/octocat/repos`, {
          params:
            {
              page: this.page,
              per_page: this.per_page
            }
        })
          .then(response => {
            // JSON responses are automatically parsed.
            this.repos = response.data
            console.log(this.repos)
          })
          .catch(e => {
            this.errors.push(e)
          })
      }

    }
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

  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
