<template>
  <div>
    <div id="error">
      <h1 v-html="header"></h1>
      <h3 v-html="subHeader"></h3>
      <img :src="errorImage" :alt="header" class="error_img" />
      <p v-html="copy"></p>
    </div>
    <footer>
      <img class="error_footer_logo" src="/Logo-Horizontal.svg" alt="Human Connection" style="max-width: 150px;" />
    </footer>
  </div>
</template>
<script>
  export default {
    props: ['error'],
    layout: 'blank',
    middleware: [
      'maintenance'
    ],
    head () {
      return {
        title: `${this.header} (${this.statusCode})`
      }
    },
    computed: {
      errorImage () {
        return `/assets/svg/errors/error${this.statusCode}.svg`
      },
      statusCode () {
        let code = 500
        const possibleErrors = [403, 404, 500, 503]
        if (possibleErrors.indexOf(this.error.statusCode) >= 0) {
          code = this.error.statusCode
        }
        return code
      },
      header () {
        return this.$t(`component.error.header${this.error.statusCode}`, `<strong>${this.error.message}</strong>`)
      },
      subHeader () {
        return this.$t(`component.error.subHeader${this.error.statusCode}`, 'How could you?')
      },
      copy () {
        return this.$t(`component.error.copy${this.statusCode}`)
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "assets/styles/_utilities";

  #error {
    margin-top: -50px;
    display: flex;
    flex-direction: column;
    // background-color: $backdrop-color;
    text-align: center;

    padding: 1.2rem;

    h1 {
      margin-bottom: 0;
      font-weight: bold;
      font-size: 2.5em;
    }
    h3 {
      margin-top: 0;
    }
    .error_img {
      min-height: 150px;
      max-height: 260px;
      padding: 3rem 2rem;
      object-fit: contain;
    }
  }

  footer {
    padding: 1rem;
    background-color: $backdrop-color;
    position: fixed;
    bottom: 0px;
    left: 0;
    text-align: center;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .button {
    margin-top: 20px;
    margin-bottom: 20px;
  }

  @media (max-height: 550px) {
    h1 {
      font-size: 1.5em;
    }
    #error {
      transform: scale(0.8);
    }
    #error .error_img {
      max-height: 25vh;
      padding: 1rem;
    }

    footer {
      padding: 0.2rem;
      height: 55px;

      .error_footer_logo {
        height: 25px;
      }
    }
  }
</style>
