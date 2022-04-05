# Vue Multi Language Web App with Vue i18n Example

### Step 1: install Vue i18n

```
npm install vue-i18n

// or

yarn add vue-i18n
```

Now update the main.js file like that.

```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import VueI18n from 'vue-i18n'
import messages from './lang'

Vue.config.productionTip = false

Vue.use(VueI18n)

export const i18n = new VueI18n({
  locale: 'en',
  fallbackLocale: 'en',
  messages
})

new Vue({
  router,
  i18n,
  render: h => h(App)
}).$mount('#app')
```

Here the variable  **messages** should contain our translations. For that, we have to create a **lang** directory.

**src/lang/index.js**

```
import en from './translations/en.json'
import fr from './translations/fr.json'

export default {
    en, fr
}
```

Update all language file like below:

**src/lang/translations/en.json**

```
{
"hello":"Hello CodeCheef",
"homePage":"Home Page",
"aboutPage":"About Page",
"welcome": "Hello {name}"
}
```

**src/lang/translations/fr.json**

```
{
"hello":"Bonjour CodeCheef",
"homePage":"Page d'accueil",
"aboutPage":"À propos de la page",
"welcome": "Bonjour {name}"
}
```

### Step 2: Configure Vue Router

In this step, we need to set up our vue router with local storage. In the router, we will check localStorage has language value or not. If don't get any value then we set a default value eng.

```
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'
import { i18n } from '../main'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

router.beforeEach((to, from, next) => {
  i18n.locale = localStorage.getItem('language') || 'en'
  return next()
})

export default router
```

Now the most important thing is that we can simply use this syntax to get output.

```
{{ $t('hello') }}
```

### Step 3: Create Language Switcher

In this step, we will create a language switch button. So update the component like below:

**src/components/Language.vue**

```
<template>
    <select v-model="$i18n.locale" @change="changeLanguage">
        <option value="en">English</option>
        <option value="fr">French</option>
    </select>
</template>

<script>
export default {
    methods:{
        changeLanguage(obj){
            localStorage.setItem('language',obj.target.value)
        }
    }
}
</script> 
```
