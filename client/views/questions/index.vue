<template>
  <div>
    <vuetable ref="vuetable"
              :api-url="apiUrl"
              :fields="fields"
              :css="css.table"
              pagination-path=""
              @vuetable:cell-dblclicked="onCellDoubleClicked"
              @vuetable:pagination-data="onPaginationData"
    >
      <template slot="actions" scope="props">
        <div class="custom-actions">
          <button class="button is-success"
                  :class="{ 'is-success': !props.rowData.moderate, 'is-danger': props.rowData.moderate, 'is-loading': loading }"
                  @click="onClick('moderate', props.rowData, props.rowIndex)">
            <i class="fa"
               :class="{ 'fa-check': !props.rowData.moderate, 'fa-close': props.rowData.moderate  }"></i>
          </button>
          <button class="button is-danger" @click="onClick('delete', props.rowData, props.rowIndex)"><i
            class="fa fa-trash"></i></button>
        </div>
      </template>
    </vuetable>

    <div class="vuetable-pagination">

      <vuetable-pagination-info ref="paginationInfo"
                                info-class="pagination-info"
      ></vuetable-pagination-info>

      <vuetable-pagination ref="pagination"
                           :css="css.pagination"
                           :icons="css.icons"
                           @vuetable-pagination:change-page="onChangePage"
      ></vuetable-pagination>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuetable from 'vuetable-2/src/components/Vuetable'
  import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
  import VuetablePaginationInfo from 'vuetable-2/src/components/VuetablePaginationInfo'
  import { allQuestionsURL } from '../../config'
  import CustomActions from './CustomActions'

  Vue.component('custom-actions', CustomActions)

  export default {
    name: 'questions',
    components: {
      Vuetable,
      VuetablePagination,
      VuetablePaginationInfo
    },

    data () {
      return {
        apiUrl: allQuestionsURL,
        loading: false,
        fields: [
          {
            name: 'title',
            title: 'Заголовок'
          },
          {
            name: 'body',
            title: 'Вопрос'
          },
          {
            name: '__slot:actions',
            title: 'Actions',
            titleClass: 'text-center',
            dataClass: 'text-center'
          }
        ],

        css: {
          table: {
            tableClass: 'table table-bordered table-striped table-hover',
            ascendingIcon: 'glyphicon glyphicon-chevron-up',
            descendingIcon: 'glyphicon glyphicon-chevron-down'
          },
          pagination: {
            wrapperClass: 'pagination',
            activeClass: 'is-active',
            disabledClass: 'disabled',
            pageClass: 'page',
            linkClass: 'link'
          },
          icons: {
            first: 'fa fa-angle-double-left',
            prev: 'fa fa-angle-left',
            next: 'fa fa-angle-right',
            last: 'fa fa-angle-double-right'
          }
        }
      }
    },

    methods: {
      onPaginationData (paginationData) {
        this.$refs.pagination.setPaginationData(paginationData)
        this.$refs.paginationInfo.setPaginationData(paginationData)
      },

      onChangePage (page) {
        this.$refs.vuetable.changePage(page)
      },

      onCellDoubleClicked (data, field, e) {
        this.$editable(e, (value) => {
          this.$http.get(allQuestionsURL + '/' + data.slug + '/' + field.name + '/' + value + '/edit')
            .then(() => console.log('question edited'))
            .catch(() => console.log('question edit error'))
        })
      },

      onClick (action, data) {
        if (action === 'moderate') {
          this.loading = true
          this.$http.get(allQuestionsURL + '/' + data.slug + '/moderate')
            .then(() => {
              this.loading = false
              this.$refs.vuetable.refresh()
            })
          return
        }

        if (action === 'delete') {
          this.$http.get(allQuestionsURL + '/' + data.slug + '/' + 'delete').then(() => this.$refs.vuetable.refresh())
        }
      }
    }

  }
</script>

<style>
  .pagination {
    margin: 0;
    float: right;
  }

  .pagination-info {
    float: left;
  }
</style>
