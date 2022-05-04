---
title: Write a Vue 3 app
date: 2022-01-22 21:17:00
updated: 2022-03-17 14:10:00
categories:
- Tech
tags:
- Web
- Vue
---

Use yarn, vite, vue, vue-router, vuex, typescript, sass etc. to write an modern web app sufer fast.

<!-- more -->

# Vue

## setup

```bash
yarn create vite my-vue-app --template vue-ts
```

## $event

```html
<vue-component @click="handle(someArg, $event)" />
```

## Watch props in Composition API

```ts
watch(() => props.someProp, (value, preValue) => {})
```

## Use element ref in Composition API

In setup function:
```ts
const refDiv = ref(null)
```
In HTML template:
```html
<div ref="refDiv"/>
```

# SCSS

Install:
```bash
yarn add sass
```

## Register global variables and mixins:

Configure vite.config.js:
```js
export default defineConfig({
    css: {
        preprocessorOptions: {
            scss: {
                additionalData: `@use "./src/style/global.scss" as *;`,
            }
        }
    },
})
```

# Vuex

## Setup

Install by
```bash
yarn add vuex@4
```

Create `src/store/index.ts`, content:
```ts
import { InjectionKey } from 'vue'
import { createStore, useStore as baseUseStore, Store } from 'vuex'

interface IState { }

export const key: InjectionKey<Store<IState>> = Symbol()

export const store = createStore<IState>({
    state: () => ({}),
    getters: {
        someGetter(state) { return 0 }
    },
    mutations: {
        someMutation(state, payload) { }
    },
    actions: {
        someAction({ state, dispatch }, payload) { }
    }
})

export function useStore() {
    return baseUseStore(key)
}
```

Add in `src/main.ts`:
```ts
...
import { key, store } from "./store"

createApp(App)
    ...
    .use(store, key)
    .mount('#app')
```

## v-model

Via setup computed.
```ts
<script lang='ts' setup>
import { computed } from 'vue'
const value = computed({
    get() { return string }
    set(newValue: string) {store.commit('setValue', value)}
})
</script>
<template>
    <input v-model="value">
</template>
```

# Element Plus

## Setup

> It seems that element-plus@2.0.1 depends on vue-router@4

Install by
```sh
yarn add element-plus
yarn add -D unplugin-vue-components unplugin-auto-import
```

Configure "vite.config.ts"
```ts
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default {
    plugins: [
        // ...
        AutoImport({
            resolvers: [ElementPlusResolver()],
        }),
        Components({
            resolvers: [ElementPlusResolver()],
        }),
      ],
}
```

# Vue Router

## Setup

Install by
```sh
yarn add vue-router@4
```

## Template

```ts
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
    {
        path: '/',
        component: () => import('../views/Home.vue'),
    },
]

const router = createRouter({
    history: createWebHashHistory(),
    routes,
    scrollBehavior(to, from, savedPosition) {
        if (to.hash) {
            return {
                el: to.hash,
                behavior: 'smooth',
            }
        }
    }
})

export default router
```

# Vite

## Set "base" for subsites

If your app is an subsite, set `base` to be `""`:

```js
export default defineConfig({
    base: '',
}
```

## Set proxy

```ts
export default defineConfig({
    ...
    server: {
        proxy: {
            '/api/': {
                target: 'https://www.somesite.com/api/v1',
                changeOrigin: true,
                rewrite: (path) => path.replace(/^\/api/, '')
            }
        }
    }
})
```

## Set alias "@"

Install dependencies:
```bash
yarn add @types/node -D
```

Modify "vite.config.js":
```js
import { resolve } from 'path'
export default defineConfig {
    // ...
    resolve: {
        alias: {
            "@": resolve(__dirname, 'src'), // 路径别名
        }
    }
    // ...
}
```

Modify "tsconfig.json"
```json
{
    "compilerOptions" : {
        // ...
        "baseUrl": ".",
        "paths": {
            "@/*": ["src/*"]
        }
        // ...
    }
}
```