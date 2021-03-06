<template>
  <section class="">
    <h1 class="is-size-3">{{ $t('component.admin.manageUsers', 'Manage Users') }}</h1>
    <br>
    <b-tabs v-model="activeTab" type="is-boxed">
      <b-tab-item label="Users">
        <no-ssr>
          <v2-table :data="users.data"
                    :loading="usersLoading"
                    :total="users.total"
                    :shown-pagination="true"
                    :pagination-info="paginationInfo"
                    @page-change="handlePageChange">
            <v2-table-column label="Name"
                             prop="name">
              <template slot-scope="row">
                <a :href="`/profile/${row.slug}`" style="white-space: nowrap;">
                  <hc-avatar :user="row" style="float: left; display: inline-block;" /> <div style="display: inline-block; float: left; padding: 5px 10px;">{{ row.name }}</div>
                </a>
              </template>
            </v2-table-column>
            <v2-table-column label="Confirmed" prop="confirmedAt">
              <template slot-scope="row">
                <i v-show="row.confirmedAt" class="fa fa-check-circle"></i>
              </template>
            </v2-table-column>
            <v2-table-column label="Role" prop="role"></v2-table-column>
          </v2-table>
        </no-ssr>
      </b-tab-item>
      <b-tab-item label="Invite Codes">
        <div class="field isGrouped columns">
          <div class="control column">
            <textarea v-model.trim="form.codes" class="textarea" rows="8" placeholder="emails"></textarea>
          </div>
          <div class="control column">
            <textarea class="textarea is-disabled" rows="8" readonly placeholder="results">{{ results }}</textarea>
          </div>
        </div>
        <div class="field">
          <div class="control">
            <hc-button color="danger"
                      @click="generateInviteCodes()"
                      :isLoading="isLoading"
                      :disabled="isLoading">
              <hc-icon set="fa" icon="magic"></hc-icon> &nbsp;<strong>{{ $t('component.admin.-', 'Generate Invite-Codes') }}</strong>
            </hc-button>
          </div>
        </div>
      </b-tab-item>
    </b-tabs>
  </section>
</template>

<script>
  import feathers from '~/plugins/feathers'
  import moment from 'moment'
  import Avatar from '~/components/Avatar/Avatar'

  let userLimit = 10

  export default {
    middleware: 'admin',
    layout: 'admin',
    components: {
      'hc-avatar': Avatar
    },
    data () {
      return {
        form: {
          codes: ''
        },
        usersLimit: userLimit,
        isLoading: false,
        usersLoading: true,
        results: '',
        users: [],
        currentPage: 1,
        paginationInfo: {
          text: this.paginationText
        },
        activeTab: null
      }
    },
    async asyncData () {
      const limit = userLimit
      const users = await feathers.service('users').find({
        query: {
          $limit: limit
        }
      })
      return {
        users: users,
        usersLoading: false,
        usersLimit: limit
      }
    },
    filters: {
      formatDate (val) {
        return moment(val).format('d.m.Y')
      }
    },
    computed: {
      totalPages () {
        return this.users.total / this.usersLimit
      },
      paginationText () {
        return `<span>Total of <strong>${this.user ? this.user.total : 0}</strong>, <strong>${this.usersLimit}</strong> per page</span>`
      }
    },
    methods: {
      generateInviteCodes () {
        const emails = this.form.codes.replace(/\n|\r|^\s+|\s+$|\s+(?=\s)|,$/g, '').split(',')

        this.isLoading = true
        this.results = ''
        feathers.service('admin').create({ createInvites: emails }).then(res => {
          this.isLoading = false
          this.$snackbar.open({
            message: this.$t('component.admin.-', 'Created new Invite-Codes'),
            duration: 4000,
            type: 'is-success'
          })
          res.forEach(item => {
            this.results += `${item.email}, ${item.code}\n`
          })
        }).catch(err => {
          console.error(err)
          this.isLoading = false
          this.$toast.open({
            message: err.message,
            duration: 3000,
            type: 'is-danger'
          })
        })
      },
      async handlePageChange (page) {
        this.currentPage = page
        this.usersLoading = true
        const start = (page - 1) * this.usersLimit + 1

        this.users = await feathers.service('users').find({
          query: {
            $limit: this.usersLimit,
            $skip: start
          }
        })
        this.usersLoading = false
      }
    },
    head () {
      return {
        title: 'Benutzer verwalten'
      }
    }
  }
</script>

<style scoped lang="scss">
</style>
