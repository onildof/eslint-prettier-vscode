Reference on setting up VS Code for Vue projects with ESLint and Prettier

//package.json
for a project with files to be interpreted by Node.js as ES Modules:
top-level field "type": "module"

for a project with files to be interpreted by Node.js as CommonJS Modules:
no "type" field

<!-- prettier-ignore -->
//.eslintrc.js
module.exports = {
  env: {
    es2021: true, //es2021 globals
    commonjs: true, //CommonJS Module globals
    node: true, //Node globals
    browser: true, //Browser globals
  },
  extends: [
    'eslint:recommended', // ESLint's built-in recommended config
    'plugin:eslint-plugin-vue/vue3-essential', // or just "plugin:vue/vue3-essential". There's also the vue3-recommended config
    'eslint-config-prettier', // or just "prettier", for disabling eslint's formatting rules that conflict with prettier
  ],
  parserOptions: {
    ecmaVersion: 'latest', // all configurations will by default support the latest ES syntax
    sourceType: 'module', // our files will have ES Module syntax
  },
  plugins: ['eslint-plugin-vue'], // or just ["vue"]
}

<!-- prettier-ignore -->
//.prettierrc.js
module.exports = {
  semi: false,
  singleQuote: true,
  htmlWhitespaceSensitivity: "ignore", //better HTML in Vue's template section
  vueIndentScriptAndStyle: true, //indent JS in Vue's script section
}

//.prettierignore
just copy .gitignore

//ESLint VS Code extension

Improved Auto Fix on Save - Auto Fix on Save is now part of VS Code's Code Action on Save infrastructure and computes all possible fixes in one round. It is customized via the editor.codeActionsOnSave setting. The setting supports the ESLint specific property source.fixAll.eslint. The extension also respects the generic property source.fixAll.
The setting below turns on Auto Fix for all providers including ESLint:

    "editor.codeActionsOnSave": {
        "source.fixAll": true
    }

In contrast, this configuration only turns it on for ESLint:

    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }

You can also selectively disable ESLint via:

    "editor.codeActionsOnSave": {
        "source.fixAll": true,
        "source.fixAll.eslint": false
    }

Mono repository setup
As with JavaScript validating TypeScript in a mono repository requires that you tell the VS Code ESLint extension what the current working directories are. Use the eslint.workingDirectories setting to do so. For this repository the working directory setup looks as follows:

    "eslint.workingDirectories": [ "./client", "./server" ]

So, in settings.json (User):
{
"editor.codeActionsOnSave": ["source.fixAll.eslint"],
"eslint.alwaysShowStatus": true,
"eslint.debug": true,
"eslint.run": "onType",
"eslint.format.enable": false,
}

//Prettier VS Code extension

Explicit ordering for Code Actions on save
You can now set editor.codeActionsOnSave to an array of Code Actions to execute in order. You can use this to guarantee that a specific Code Action is always run before or after another one that may conflict with it.

The following editor.codeActionsOnSave will always run Organize Imports followed by Fix All once organize imports finishes:

"editor.codeActionsOnSave": [
"source.organizeImports",
"source.fixAll"
]

So, in settings.json (User):
{
"editor.defaultFormatter": "esbenp.prettier-vscode",
"editor.formatOnSave": true,
"editor.formatOnPaste": true,
}

todo:

- rohit-gohri.format-code-action
- .prettierignore
- .eslintignore
- jsconfig.json
- more vue configs
- volar
- monorepo adjustments
