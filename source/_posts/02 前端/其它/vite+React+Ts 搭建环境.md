# vite+React+Ts搭建环境

## 配置eslint和prettier

`yarn add eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin -D`

创建.eslintrc.js

```jsx
module.exports = {
	parser: '@typescript-eslint/parser',
	parserOptions: {
		project: 'tsconfig.json',
		sourceType: 'module',
	},
	plugins: ['@typescript-eslint/eslint-plugin'],
	extends: [
		'plugin:@typescript-eslint/recommended',
		'plugin:prettier/recommended',
	],
	root: true,
	env: {
		node: true,
		jest: true,
	},
	ignorePatterns: ['.eslintrc.js'],
	rules: {
		'@typescript-eslint/interface-name-prefix': 'off',
		'@typescript-eslint/explicit-function-return-type': 'off',
		'@typescript-eslint/explicit-module-boundary-types': 'off',
		'@typescript-eslint/no-explicit-any': 'off',
	},
};
```

`yarn add prettier -D`

创建.prettierrc

```json
{
	"singleQuote": true,
	"trailingComma": "all"
}
```

## 引入AntDesign

先安装antdesign 和 icons 依赖包

`yarn add` `antd @ant-design/icons -S`

### 全局引入

打开main.tsx添加antd样式

```css
...
import 'antd/dist/antd.css'
...
```

回到App.tsx,引入Button组件测试

```tsx
import { Button } from 'antd'
function App() {

  return (
    <div className="App"> 
    <Button type='primary'>Test</Button>
    </div>
  )
}
export default App
```

### 按需引入

`yarn add vite-plugin-imp -D`

`yarn add less -D`

修改vit.config.ts文件

```tsx
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import vitePluginImp from 'vite-plugin-imp'
// <https://vitejs.dev/config/>
export default defineConfig({
  plugins: [
    react(),
    vitePluginImp({
      libList: [
        {
          libName: "antd",
          style: (name) => `antd/lib/${name}/style/index.less`,
        },
      ],
    })
  ],
  css: {
    preprocessorOptions: {
      less: {
        // 支持内联 JavaScript
        javascriptEnabled: true,
        modifyVars: {
          '@primary-color': '#4377FE',//设置antd主题色
        },
      }
    }
  },
})
```

## 引入TailWindCSS

[tailwindcss指引](https://tailwindcss.com/docs/guides/vite)

`yarn add -D tailwindcss postcss autoprefixer`

`npx tailwindcss init -p`

配置tailwind.config.js

```tsx
/** @type {import('tailwindcss').Config} */ 
module.exports = {
  content: [
    "./index.html",
    "./src/**/*.{vue,js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
//与antdesign样式冲突
	corePlugins: {
    preflight: false
  }
}
```

修改index.css并将其在main.tsx中引入

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

<aside> 💡 Tips

此时在vscode环境中会显示

Unknown at rule @tailwind 警告

Unknown at rule @components 警告

Unknown at rule @utilities 警告

此时需要安装 [PostCSS Language Support](https://marketplace.visualstudio.com/items?itemName=csstools.postcss) 插件

</aside>

```tsx
//main.tsx
...
import './index.css'
...
```

## 在vite中配置alias和server

### 配置alias

```tsx
//vite.config.ts
...
resolve: {
    alias: [{
      find: '@',
      replacement: path.resolve(__dirname, 'src')
    },
    {
      find: '@/pages',
      replacement: path.resolve(__dirname, 'src/pages')
    }
    ]
  },
...
```

修改tsconfig.json

```tsx
"compilerOptions": {
...
"baseUrl": "./",
    "paths": {
      "@/*": [
        "src/*"
      ]
    }
...
}
```

配置server

```tsx
server: {
    host: 'localhost',
    port: 3000,
    cors: true,
    https: false,
    proxy: {
      '/api': {
        target: '<http://localhost:8080>',
        ws: false,
        changeOrigin: true,
        rewrite: (path: string) => path.replace(/^\\/api/, '')
      }
    }
  }
```

