---
title: vue3+vite+ts
date: 2021-05-09 19:46:52
tags:
categories: vue3
---

# 项目模版

[vue3-template](https://github.com/Sorryjjj/vue3-template.git)

# vite

[vite](https://cn.vitejs.dev/guide/)

创建项目模版

```sh
yarn create @vitejs/app vite-template --template vue-ts
```

# jest

[jest](https://www.jestjs.cn/docs/getting-started)

```sh
yarn add jest --dev // 安装jest
yarn add @types/jest --dev  // jest提示
yarn add babel-jest --dev   // babel转换
yarn add @babel/preset-env --dev // babel预设
yarn add vue-jest@next --dev // 解析.vue
yarn add @vue/test-utils@next --dev // 安装vue/test-utils
yarn add @babel/preset-typescript --dev // babel ts
yarn add ts-jest --dev // 安装ts-test
```

```js
// jest.config.js
module.exports = {
    transform: {
        "^.+\\.jsx?$": "babel-jest",
      	"^.+\\.vue?$": "vue-jest",
      	"^.+\\.tsx?$": "ts-jest"
    }
}
```

```js
// babel.config.js
module.exports = {
    presets: [
        ['@babel/preset-env', { targets: { node: 'current' } }], 
        "@babel/preset-typescript"
    ],
}
```

# gitHooks

```sh
yarn add yorkie --dev
yarn add chalk --dev
```

```json
  "gitHooks": {
    "pre-commit": "lint-staged",
    "commit-msg": "node scripts/verifyCommit.js"
  },
  "lint-staged": {
    "*.{ts,vue}": "eslint --fix",
    "*": "prettier -w -u"
  }
```

# eslint

```sh
yarn add -D eslint eslint-plugin-vue @vue/eslint-config-typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin typescript
yarn add lint-staged
yarn add -D prettier eslint-plugin-prettier @vue/eslint-config-prettier
```

```js
// .eslintrc.js
module.exports = {
  root: true,
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  extends: [
    'plugin:vue/vue3-recommended',
    'eslint:recommended',
    '@vue/typescript/recommended',
    '@vue/prettier',
    '@vue/prettier/@typescript-eslint',
  ],
  parserOptions: {
    ecmaVersion: 2021,
  },
  plugins: [],
  rules: {},
}
```

# alias

```js
// vite.config.ts
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import { join } from "path";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: [
      {
        find: "@/",
        replacement: join(__dirname, "src/"),
      },
    ],
  },
});

```

