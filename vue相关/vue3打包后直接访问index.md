# vite+vue3 打包后如何直接打开index.html文件

### 1.根据环境配置vite.config.ts配置：主要的就是base: env.VITE_MODE === 'production' ? './' : '/',

```
import { defineConfig, loadEnv } from 'vite'
import vue from '@vitejs/plugin-vue'
import pxtovw from 'postcss-px-to-viewport'
import vueJsx from '@vitejs/plugin-vue-jsx'
import path from 'path'
// import { createStyleImportPlugin, NutuiResolve } from 'vite-plugin-style-import'

const loder_pxtovw = pxtovw({
  viewportWidth: 414,
  viewportUnit: 'vw'
})

// https://vitejs.dev/config/
export default defineConfig(({ mode }) => {
  // 获取当前环境的配置
  const config = loadEnv(mode, './')
  return {
    plugins: [
      vue(),
      vueJsx({})
      // createStyleImportPlugin({
      //   resolves: [
      //     NutuiResolve(),
      //   ]
      // }),
    ],
    //根据环境加载文件
    base: env.VITE_MODE === 'production' ? './' : '/',
    resolve: {
      alias: {
        '@': path.resolve('src')
      }
    },
    css: {
      preprocessorOptions: {
        less: {
          charset: false,
          additionalData: '@import "./src/style/global.less";' // 加载全局样式，使用less特性
        }
      },
      postcss: {
        plugins: [loder_pxtovw]
      }
    },
    server: {
      host: '127.0.0.1',
      port: 5173, //指定端口号
      open: true,
      proxy: {
        [config.VITE_WATER_SPIRIT_CROS]: {
          target: config.VITE_WATER_SPIRIT_URL,
          changeOrigin: true,
          rewrite: (path) => path.replace(/^\/api/, '')
        },
        [config.VITE_PT_CROS]: {
          target: config.VITE_PT_URL,
          changeOrigin: true,
          rewrite: (path) => path.replace(/^\/planycom/, '')
        }
      }
    }
  }
})

```

### 2.打包后的结果如图所示，文件路径是./ 其实 去掉./也是可以的，但是打包后是/favicon.ico这种路径是访问不到的，参考第一部分

![img](https://img-blog.csdnimg.cn/f79816807ead4c6989eac862964dcb84.png)

### 3.路由改成 createWebHashHistory 

![img](https://img-blog.csdnimg.cn/e840a349eb6d4abdb9bf9ec8ed055eb0.png)

### 4.vscode安装Live Server

![img](https://img-blog.csdnimg.cn/39f42540c48e401a9c10c7af73803a84.png)

`关于vite build后访问报错：Expected a JavaScript module script but the server responded with a MIME type of`

直接将地址谁知为base 设置成 './'  