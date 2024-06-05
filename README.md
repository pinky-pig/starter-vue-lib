# starter-vue-lib

- vite vue vueuse vue-router
- shadcn-vue
- tailwindcss
- vitepress
- vite-lib npm package

## 开发

开发 lib 是在 `package/` 目录下，运行项目测试是在 `src/` 目录下，Vitepress 文档在 `docs/` 目录下。

`docs/` 可以直接使用 `src/components` 中的组件，因为在 `docs/.vitepress/config.mts` 中配置了 `unplugin-vue-components/vite` 和 `unplugin-auto-import/vite` ，不过并没有配置 tsconfig.json 覆盖根目录中的。

### 安装依赖

```bash
pnpm install -r
```

### 启动项目

```bash
npm run dev
```

### 引用包

```js
import foo from '../../packages/foo'
```

### 1. 测试本地包（workspace）

1. 在 packages 下新建文件

2. 根目录新建 `pnpm-workspace.yaml` 文件，并写入

```yaml
packages:
  - 'packages/**'
```

3. 安装全局的包，使用 `-w` ， 安装到 packages 中的包，使用 `-r filter` ， 这个可以参考官方文档。 `-r` 是遍历文件夹 recurse（递归）。

```bash
# 安装到单个 packages
pnpm i dayjs -r --filter @test/web
```

4. 使用

在根项目中使用，直接这样安装：

```bash
# 这个包已经在 packages 中声明
pnpm i @arvin/materials -w
```

然后在组件中使用

```js
import { AnimatedTooltip } from '@arvin/materials'
```

**避免使用 workspace 中的包**

```bash
# foo 是包的名字
pnpm --ignore-workspace install foo
```

### 2. 测试本地包(link)

<https://pnpm.io/zh/next/cli/link>

```bash
# 进入包目录
cd ~/packages
# 链接到全局
pnpm link --global
# 退到根目录
cd ..
# 安装本地包
pnpm link --global foo
```

就会发现 node_modules 中有个 foo 包了。

但这个时候引入的时候，会有 ts 提示找不到，所以我们需要配置一些 tsconfig.json

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "foo": ["node_modules/foo/index.ts"]
    }
  }
}
```

## 打包 lib

### 构建

会打包 js 和 css 

```bash
npm run build:lib
```

### 发包

会修改并拷贝 package.json，拷贝 README.md

```bash
npm run release:lib
```

## 打包 docs

### 构建

```bash
npm run docs:dev
```

### 打包

```bash
npm run docs:build
```

### 预览

```bash
npm run docs:preview
```

### 打包后的文件

`/docs/.vitepress/dist`

# 使用

使用此仓库后，如果需要合本仓库的最新代码，添加远程仓库后，拉取代码。

```bash
git pull --tags starter-vue-lib main --allow-unrelated-histories
```
