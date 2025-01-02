# Vite 配置方法

## base

```js
export default defineConfig(({mode}) => {
    return {
        base: './',
        plugins: [vue()],
    }
})
```

## alias

```js
export default defineConfig(({mode}) => {
    return {
        plugins: [vue()],
        resolve: {
            alias: {
                '@': resolve(__dirname, 'src')
            }
        }
    }
})
```

## 生产环境移除console

```js
export default defineConfig(({mode}) => {
    return {
        plugins: [vue()],
        build: {
            minify: "terser",
            terserOptions: {
                compress: {
                    // 生产环境移除console
                    drop_console: true,
                    drop_debugger: true
                }
            }
        }
    }
})
```

## 配置proxy代理

```js
export default defineConfig(({mode}) => {
    return {
        plugins: [vue()],
        server: {
            proxy: {
                '/api': {
                    target: loadEnv(mode, process.cwd()).VITE_BASE_API,
                    changeOrigin: true,
                    rewrite: (path) => path.replace(/^\/api/, ''),
                },
            }
        },
    }
})

```

## env环境变量配置

创建 .env.development 和 .env.production 两个文件。

在文件中用VITE_XXX作为变量。