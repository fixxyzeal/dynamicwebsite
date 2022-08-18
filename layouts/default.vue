<template>
  <v-app light>
    <v-navigation-drawer v-model="drawer" :mini-variant="miniVariant" :clipped="clipped" fixed app>
      <v-list>
        <v-list-item v-for="(item, i) in items" :key="i" :to="item.to" router exact>
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app color="primary" :class="textwhite">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" :class="textwhite" />

      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn v-for="(button,i) in navbuttons" :key="i" :class="button.color" :href="button.link" icon target="_blank">
        <v-icon>{{button.icon}}</v-icon>
      </v-btn>

      <v-menu offset-y>
        <template v-slot:activator="{ on: menu, attrs }">
          <v-btn large color="primary" dark v-bind="attrs" v-on="{ ...menu }">
            {{ $i18n.locale }}
          </v-btn>
        </template>
        <v-list>
          <v-list-item v-for="item in availableLocales" :key="item.code" :to="switchLocalePath(item.code)">
            <v-list-item-title class="text-center">{{ item.code.toUpperCase() }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <v-footer :absolute="!fixed" app color="primary" :class="textwhite">
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      clipped: true,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
      ],
      navbuttons:[
        { icon: 'mdi-facebook', link: 'https://www.facebook.com/MTinterLabClinic', color:'white--text'},
        { icon: 'mdi-instagram', link: 'https://www.instagram.com/m.t.inter.lab_official', color:'white--text' },
      ],
      miniVariant: false,
      right: false,
      rightDrawer: false,
      title: 'Dynamic Website',
      textwhite: 'white--text',
    }
  },
  computed: {
    availableLocales() {
      return this.$i18n.locales.filter((i) => i.code !== this.$i18n.locale)
    },
    menus() {
      this.items = [
        {
          icon: 'mdi-hospital-building',
          title: this.$t('menu').aboutus,
          to: this.$i18n.locale == 'en' ? '/en' : '/',
        },
        {
          icon: 'mdi-hospital-box-outline',
          title: this.$t('menu').package,
          to: (this.$i18n.locale == 'en' ? '/en/' : '/') + 'package',
        },
        {
          icon: 'mdi-calendar-text-outline',
          title: this.$t('menu').appointment,
          to: (this.$i18n.locale == 'en' ? '/en/' : '/') + 'appointment',
        },
        {
          icon: 'mdi-card-account-phone-outline',
          title: this.$t('menu').contact,
          to: (this.$i18n.locale == 'en' ? '/en/' : '/') + 'contactus',
        },
        {
          icon: 'mdi-facebook',
          title: this.$t('menu').facebook,
          to: (this.$i18n.locale == 'en' ? '/en/' : '/') + 'facebook',
        },
      ]
      return this.items
    },
  },
  mounted() {
    //Get button io
    var options = {
      facebook: '1396833660467510', // Facebook page ID
      line: '//lin.ee/nirFVid', // Line QR code URL
      call_to_action: 'Contact us directly', // Call to action
      button_color: '#AB47BC', // Color of button
      position: 'right', // Position may be 'right' or 'left'
      order: 'facebook,line', // Order of buttons
    }
    var proto = document.location.protocol,
      host = 'getbutton.io',
      url = proto + '//static.' + host
    var s = document.createElement('script')
    s.type = 'text/javascript'
    s.async = true
    s.src = url + '/widget-send-button/js/init.js'
    s.onload = function () {
      WhWidgetSendButton.init(host, proto, options)
    }
    var x = document.getElementsByTagName('script')[0]
    x.parentNode.insertBefore(s, x)
  },
}
</script>
<style scoped>
.v-application {
  background-color: #e8e8e8;
}
</style>
