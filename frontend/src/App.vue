<template>
  <div
    id="app"
    class="app"
  >
    <header
      v-if="$store.getters.isAuthenticated"
      class="app__header"
    >
      <router-link
        class="app__header-link"
        :to="{name: 'home'}"
      >
        {{ $t('home.linkTitle') }}
      </router-link>
      <router-link
        class="app__header-link"
        :to="{name: 'persons-overview'}"
      >
        {{ $t('persons.linkTitle') }}
      </router-link>
      <router-link
        class="app__header-link"
        :to="{name: 'locations-overview'}"
      >
        {{ $t('locations.linkTitle') }}
      </router-link>
      <router-link
        class="app__header-link"
        :to="{name: 'relations-overview'}"
      >
        {{ $t('relations.linkTitle') }}
      </router-link>
      <router-link
        class="app__header-link"
        :to="{name: 'addresses-overview'}"
      >
        {{ $t('addresses.linkTitle') }}
      </router-link>
      <router-link
        class="app__header-link"
        :to="{name: 'relationTypes-overview'}"
      >
        {{ $t('relationTypes.linkTitle') }}
      </router-link>
      <router-link
        class="app__header-link"
        :to="{name: 'locationTypes-overview'}"
      >
        {{ $t('locationTypes.linkTitle') }}
      </router-link>
      <router-link
        class="app__header-link"
        :to="{name: 'routes-overview'}"
      >
        {{ $t('routes.linkTitle') }}
      </router-link>
      <DataLanguageSelect class="app__data-language-select" />
    </header>
    <router-view :key="$route.fullPath" />
  </div>
</template>

<script>
  import axios from 'axios';
  import DataLanguageSelect from './components/DataLanguageSelect';

  export default {
    name: 'App',
    components: {
      DataLanguageSelect
    },
    created() {
      if (this.$store.state.auth.accessToken) {
        axios.defaults.headers.common['Authorization'] = `Bearer ${this.$store.state.auth.accessToken}`;
      }

      this.$store.watch(
        state => state.auth.accessToken,
        accessToken => {
          if (!accessToken) {
            delete axios.defaults.headers.common['Authorization'];
            this.$router.push('/login');
          } else {
            axios.defaults.headers.common['Authorization'] = `Bearer ${accessToken}`;
          }
        }
      );

      this.$store.watch(
        state => state.app.interfaceLanguage,
        newLang => {
          this.$i18n.i18next.changeLanguage(newLang);
        },
        {
          immediate: true
        }
      );
    }
  };
</script>

<style src="./styles/base.css"></style>

<style>
  @import "~vue2-dropzone/dist/vue2Dropzone.min.css";

  .app {
    &__header {
      display: flex;
      align-items: center;
      height: 50px;
      border-bottom: 1px solid black;
    }

    &__header-link {
      color: blue;
      margin-left: 20px;
      text-decoration: none;
    }

    &__data-language-select {
      margin-left: auto;
    }

    .router-link-active {
      text-decoration: underline;
    }
  }
</style>
