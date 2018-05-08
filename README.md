# npm-or-cnpm
你该如何选择npm还是cnpm？

# 什么是npm?
* npm 为你和你的团队打开了连接整个 JavaScript 天才世界的一扇大门。它是世界上最大的软件注册表，每星期大约有 30 亿次的下载量，包含超过 600000 个 包（package） （即，代码模块）。来自各大洲的开源软件开发者使用 npm 互相分享和借鉴。包的结构使您能够轻松跟踪依赖项和版本。(官方文档上写的）
* 自己其实就是可以理解为一个挺大的小工具库，而且还能轻松的查看依赖项和版本，世界各地的牛人互相借鉴的开放库

# 什么是cnpm？
* 这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。(官网说的)
* 很少有人去看这个，大家记住10分钟同步一次！

# 在我们中国，要下载 npm 包非常慢，如果使用 cnpm 下载包就非常快了，感觉很爽，但是 cnpm 也有几个大问题：
1. cnpm 的仓库只是 npm 仓库的一个拷贝，它不承担 publish 工作，所以你用 cnpm publish 命令会执行失败的（在cnpm官网上最后有说）
2. 不仅是 publish 会执行失败，其它的需要注册用户(npm adduser)、或者修改 package 状态等命令都无法用 cnpm
3. 有很多 npm 包都集成了 npm install，比如 yeoman 的所有 generators ，在最后基本都会调用 npm install，（ 可以看其源码 ）这种情况下使用 cnpm 完全无效，必须中断操作，然后自己手动运行 cnpm install，或者在运行 yo [generator] 时就指定 --skip-install，这体验就很不爽了
4. 还有一种情况是，很多和 npm API 相关的 package，都会读取 ~/.npmrc 中的 registry，或者使用默认的 registry —— [https://registry.npmjs.org/][npm-registry]，去查询 npm package 相关的信息，比如下面这些：
- npm-latest: 查询某个 package 的最新版本号
- npm-name: 查询某个 package name 是否被注册了
- npm-dependents: 查找某个模块所依赖的其它所有模块
- …
- 如果你用的任何一个包或其所依赖的包中用了这些 package，那么在这些包请求网络时也得慢死了！

# 项目中遇到过的cnpm的坑总结
1. 做react native开发 用 cnpm 装的包都是在 node_modules 文件夹下以 版本号 @包名 命名的，然后再做软链接到只以包名命名的文件夹上，导致的问题就是启动 packager 时找不到路径
2. 直接从没有 node_modules 文件夹时 cnpm install ， webstorm 就完全卡住死掉了。。。只能一个一个安装，每次等建立完索引再安装下一个

- 解决办法：CNPM 的 link 方案确实快,但是 rn 里面还是用 npm 比较好, 只用淘宝的源就可以了 
<pre> 
 - npm i react-native --registry=https://registry.npm.taobao.org 
 - 在.npmrc 里添加 registry=https://registry.npm.taobao.org 
</pre>
3. 还出现过cnpm下载依赖不完整的现象
- 解决办法： 使用npm重新下在

# 使用一些小技巧
1. 使用cnpm代替npm
<pre>
  1.  npm install -g cnpm --registry=https://registry.npm.taobao.org
  2.  alias cnpm="npm --registry=https://registry.npm.taobao.org \
    --cache=$HOME/.npm/.cache/cnpm \
    --disturl=https://npm.taobao.org/dist \
    --userconfig=$HOME/.cnpmrc"

    # Or alias it in .bashrc or .zshrc
    $ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
      --cache=$HOME/.npm/.cache/cnpm \
      --disturl=https://npm.taobao.org/dist \
      --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
</pre>
2. 直接使用npm 但是内部使用淘宝镜像
<pre>
  npm config set registry https://registry.npm.taobao.org
</pre>

# 最后个人建议
. 在网络可以或者能够成功翻墙的情况下尽量使用 npm
. 不要同时使用npm和cnpm
. 在使用了Webstorm这类的IDE 尽量使用npm
- 以上只是个人建议少出现奇葩问题
. 联系方式：qq：929364695  手机：18610760206或15201582062
