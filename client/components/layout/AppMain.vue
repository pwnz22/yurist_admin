<template>
  <section class="app-main">
    <vuetable ref="vuetable"
              api-url="http://yurist-api.ils-team.ru/api/admin/questions"
              :fields="fields"
    ></vuetable>
    <!--<div class="container is-fluid is-marginless app-content">-->
    <!--<levelbar></levelbar>-->
    <!--<transition-->
    <!--mode="out-in"-->
    <!--enter-active-class="fadeIn"-->
    <!--leave-active-class="fadeOut"-->
    <!--appear>-->
    <!--<router-view class="animated"></router-view>-->
    <!--</transition>-->
    <!--</div>-->
  </section>
</template>

<script>
  import Vuetable from 'vuetable-2/src/components/Vuetable'
  import Levelbar from './Levelbar'

  export default {
    components: {
      Levelbar,
      Vuetable
    },
    data () {
      return {
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

<style lang="scss">
  @import '~bulma/sass/utilities/variables';
  @import '~bulma/sass/utilities/mixins';

  .app-main {
    padding-top: 50px;
    margin-left: 180px;
    transform: translate3d(0, 0, 0);

    @include mobile() {
      margin-left: 0;
    }

  }

  .app-content {
    padding: 20px;
  }
</style>
