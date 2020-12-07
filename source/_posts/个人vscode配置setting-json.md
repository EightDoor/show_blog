---
title: 个人vscode配置setting.json
date: 2020-01-14 10:31:57
tags: 前端
categories: 其他
---

{
"workbench.iconTheme": "material-icon-theme",
"editor.fontSize": 18,
//字体文件
"editor.fontFamily": "JetBrains Mono",
"editor.formatOnSave": true,
"java.configuration.checkProjectSettingsExclusions": false,
"typescript.updateImportsOnFileMove.enabled": "never",
"cSpell.enabledLanguageIds": [
"asciidoc",
"c",
"cpp",
"csharp",
"css",
"git-commit",
"go",
"handlebars",
"haskell",
"html",
"jade",
"java",
"javascriptreact",
"json",
"jsonc",
"latex",
"less",
"markdown",
"php",
"plaintext",
"pug",
"python",
"restructuredtext",
"rust",
"scala",
"scss",
"text",
"typescript",
"typescriptreact",
"yaml",
"yml"
],
"javascript.updateImportsOnFileMove.enabled": "always",
"[typescriptreact]": {},
"editor.tabSize": 2,
"editor.detectIndentation": false, // 全部统一成 2 空格缩进
"eslint.enable": true, //是否开启 vscode 的 eslint
"editor.codeActionsOnSave": {
"source.fixAll.eslint": true
},
"eslint.options": { //指定 vscode 的 eslint 所处理的文件的后缀
"extensions": [
".js",
".jsx",
".ts",
".tsx"
]
},
"eslint.validate": [ //确定校验准则
"javascript",
"javascriptreact",
"html",
"typescript",
"typescriptreact"
],
"search.followSymlinks": false,
"files.exclude": {
"**/.git": true,
"**/.svn": true,
"**/.hg": true,
"**/CVS": true,
"**/.DS_Store": true,
"**/tmp": true,
// "**/node_modules": true,
"**/bower_components": true,
// "**/dist": true
},
"files.watcherExclude": {
"**/.git/objects/**": true,
"**/.git/subtree-cache/**": true,
// "**/node_modules/**": true,
"**/tmp/**": true,
"**/bower_components/**": true,
"**/dist/\*\*": true
},
"explorer.confirmDelete": false
}
