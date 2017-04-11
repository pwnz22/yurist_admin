<template>
  <div>
    <vuetable ref="vuetable"
              :api-url="apiUrl"
              :fields="fields"
    ></vuetable>
  </div>
</template>

<script>
  import Vuetable from 'vuetable-2/src/components/Vuetable'
  import { allQuestionsURL } from '../../config'

  export default {
    name: 'questions',
    components: {
      Vuetable
    },

    data () {
      return {
        apiUrl: allQuestionsURL,
        fields: [
          {
            name: 'title',
            title: 'Заголовок'
          }
        ]
      }
    }
  }
</script>
