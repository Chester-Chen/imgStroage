date: 2020.01.19

# npm安装node-sass相关错误的解决办法

在使用vue-element-admin时，到这一步时`npm install`，五花八门的错误。刚开始的python2找不到，于是我就卸载了python3装个py2，
这里很奇怪py3竟然不向下兼容。然后，node-sass这一步又是各种报错，主要是源的问题。网上看了一堆资料，vue-element-admin官方文
档也是给了温馨提示，叫我们不要使用cnpm或者用yarn。下面是最后成功步骤，并附上报错。

```
npm WARN deprecated runjs@4.4.2: This project has been renamed to 'tasksfile'. Install using 'npm install tasksfile' instead.
npm WARN deprecated core-js@2.6.11: core-js@<3 is no longer maintained and not recommended for usage due to the number of issues. Please, upgrade your dependencies to the actual version of core-js@3.
npm WARN deprecated microcli@1.3.3: This project has been renamed to @pawelgalazka/cli . Install using @pawelgalazka/cli instead
npm WARN deprecated nomnom@1.8.1: Package no longer supported. Contact support@npmjs.com for more info.
npm WARN deprecated microargs@1.1.2: This project has been renamed to @pawelgalazka/cli-args. Install using @pawelgalazka/cli-args instead
npm WARN deprecated circular-json@0.3.3: CircularJSON is in maintenance only, flatted is its successor.
npm WARN deprecated kleur@2.0.2: Please upgrade to kleur@3 or migrate to 'ansi-colors' if you prefer the old syntax. Visit <https://github.com/lukeed/kleur/releases/tag/v3.0.0\> for migration path(s).
npm WARN deprecated left-pad@1.3.0: use String.prototype.padStart()

> yorkie@2.0.0 install D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\yorkie
> node bin/install.js

setting up Git hooks
can't find .git directory, skipping Git hooks installation

> husky@1.3.1 install D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\husky
> node husky install

husky > setting up git hooks
Can't find .git, skipping Git hooks installation.
Please check that you're in a cloned repository or run 'git init' to create an empty Git repository and reinstall husky.

> node-sass@4.13.1 install D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass
> node scripts/install.js

Downloading binary from https://github.com/sass/node-sass/releases/download/v4.13.1/win32-x64-64_binding.node
Cannot download "https://github.com/sass/node-sass/releases/download/v4.13.1/win32-x64-64_binding.node":

connect ETIMEDOUT 52.216.200.155:443

Timed out whilst downloading the prebuilt binary

Hint: If github.com is not accessible in your location
      try setting a proxy via HTTP_PROXY, e.g.

      export HTTP_PROXY=http://example.com:1234

or configure npm proxy via

      npm config set proxy http://example.com:8080

> core-js@2.6.11 postinstall D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\core-js
> node -e "try{require('./postinstall')}catch(e){}"

Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!

The project needs your help! Please consider supporting of core-js on Open Collective or Patreon:
> https://opencollective.com/core-js
> https://www.patreon.com/zloirock

Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)


> ejs@2.7.4 postinstall D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\ejs
> node ./postinstall.js

Thank you for installing EJS: built with the Jake JavaScript build tool (https://jakejs.com/)


> node-sass@4.13.1 postinstall D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass
> node scripts/build.js

Building: C:\Program Files\nodejs\node.exe D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-gyp\bin\node-gyp.js rebuild --verbose --libsass_ext= --libsass_cflags= --libsass_ldflags= --libsass_library=
Active code page: 65001
gyp info it worked if it ends with ok
gyp verb cli [ 'C:\\Program Files\\nodejs\\node.exe',
gyp verb cli   'D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\node-gyp\\bin\\node-gyp.js',
gyp verb cli   'rebuild',
gyp verb cli   '--verbose',
gyp verb cli   '--libsass_ext=',
gyp verb cli   '--libsass_cflags=',
gyp verb cli   '--libsass_ldflags=',
gyp verb cli   '--libsass_library=' ]
gyp info using node-gyp@3.8.0
gyp info using node@10.15.0 | win32 | x64
gyp verb command rebuild []
gyp verb command clean []
gyp verb clean removing "build" directory
gyp verb command configure []
gyp verb check python checking for Python executable "python2" in the PATH
gyp verb `which` failed Error: not found: python2
gyp verb `which` failed     at getNotFoundError (D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\which\which.js:13:12)
gyp verb `which` failed     at F (D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\which\which.js:68:19)
gyp verb `which` failed     at E (D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\which\which.js:80:29)
gyp verb `which` failed     at D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\which\which.js:89:16
gyp verb `which` failed     at D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\isexe\index.js:42:5
gyp verb `which` failed     at D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\isexe\windows.js:36:5
gyp verb `which` failed     at FSReqWrap.oncomplete (fs.js:154:21)
gyp verb `which` failed  python2 { Error: not found: python2
gyp verb `which` failed     at getNotFoundError (D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\which\which.js:13:12)
gyp verb `which` failed     at F (D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\which\which.js:68:19)
gyp verb `which` failed     at E (D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\which\which.js:80:29)
gyp verb `which` failed     at D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\which\which.js:89:16
gyp verb `which` failed     at D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\isexe\index.js:42:5
gyp verb `which` failed     at D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\isexe\windows.js:36:5
gyp verb `which` failed     at FSReqWrap.oncomplete (fs.js:154:21)
gyp verb `which` failed   stack:
gyp verb `which` failed    'Error: not found: python2\n    at getNotFoundError (D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\which\\which.js:13:12)\n    at F (D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\which\\which.js:68:19)\n    at E (D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\which\\which.js:80:29)\n    at D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\which\\which.js:89:16\n    at D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\isexe\\index.js:42:5\n    at D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\isexe\\windows.js:36:5\n    at FSReqWrap.oncomplete (fs.js:154:21)',
gyp verb `which` failed   code: 'ENOENT' }
gyp verb check python checking for Python executable "python" in the PATH
gyp verb `which` succeeded python C:\Python27\python.EXE
gyp verb check python version `C:\Python27\python.EXE -c "import sys; print "2.7.17
gyp verb check python version .%s.%s" % sys.version_info[:3];"` returned: %j
gyp verb get node dir no --target version specified, falling back to host node version: 10.15.0
gyp verb command install [ '10.15.0' ]
gyp verb install input version string "10.15.0"
gyp verb install installing version: 10.15.0
gyp verb install --ensure was passed, so won't reinstall if already installed
gyp verb install version is already installed, need to check "installVersion"
gyp verb got "installVersion" 9
gyp verb needs "installVersion" 9
gyp verb install version is good
gyp verb get node dir target node version installed: 10.15.0
gyp verb build dir attempting to create "build" dir: D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build
gyp verb build dir "build" dir needed to be created? D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build
gyp verb Not using VS2017: Could not use PowerShell to find VS2017
gyp verb build/config.gypi creating config file
gyp verb build/config.gypi writing out config file: D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\config.gypi
gyp verb config.gypi checking for gypi file: D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\config.gypi
gyp verb common.gypi checking for gypi file: D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\common.gypi
gyp verb gyp gyp format was not specified; forcing "msvs"
gyp info spawn C:\Python27\python.EXE
gyp info spawn args [ 'D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\node-gyp\\gyp\\gyp_main.py',
gyp info spawn args   'binding.gyp',
gyp info spawn args   '-f',
gyp info spawn args   'msvs',
gyp info spawn args   '-G',
gyp info spawn args   'msvs_version=auto',
gyp info spawn args   '-I',
gyp info spawn args   'D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\node-sass\\build\\config.gypi',
gyp info spawn args   '-I',
gyp info spawn args   'D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\node-gyp\\addon.gypi',
gyp info spawn args   '-I',
gyp info spawn args   'C:\\Users\\26305\\.node-gyp\\10.15.0\\include\\node\\common.gypi',
gyp info spawn args   '-Dlibrary=shared_library',
gyp info spawn args   '-Dvisibility=default',
gyp info spawn args   '-Dnode_root_dir=C:\\Users\\26305\\.node-gyp\\10.15.0',
gyp info spawn args   '-Dnode_gyp_dir=D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\node-gyp',
gyp info spawn args   '-Dnode_lib_file=C:\\Users\\26305\\.node-gyp\\10.15.0\\<(target_arch)\\node.lib',
gyp info spawn args   '-Dmodule_root_dir=D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\node-sass',
gyp info spawn args   '-Dnode_engine=v8',
gyp info spawn args   '--depth=.',
gyp info spawn args   '--no-parallel',
gyp info spawn args   '--generator-output',
gyp info spawn args   'D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\node-sass\\build',
gyp info spawn args   '-Goutput_dir=.' ]
gyp verb command build []
gyp verb build type Release
gyp verb architecture x64
gyp verb node dev dir C:\Users\26305\.node-gyp\10.15.0
gyp verb found first Solution file build/binding.sln
gyp verb could not find "msbuild.exe" in PATH - finding location in registry
gyp info spawn C:\Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe
gyp info spawn args [ 'build/binding.sln',
gyp info spawn args   '/nologo',
gyp info spawn args   '/p:Configuration=Release;Platform=x64' ]
在此解决方案中一次生成一个项目。若要启用并行生成，请添加“/m”开关。
生成启动时间为 2020/1/19 20:13:15。
节点 1 上的项目“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\binding.sln”(默认目标)。
ValidateSolutionConfiguration:
  正在生成解决方案配置“Release|x64”。
项目中不存在 BeforeTargets 特性中的“C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\BuildCustomizations\masm.targets (28,5)”位置列出的目标“Midl”，将忽略该目标。
项目中不存在 AfterTargets 特性中的“C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\BuildCustomizations\masm.targets (29,5)”位置列出的目标“CustomBuild”，将忽略该目标。
项目“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\binding.sln”(1)正在节点 1 上生成“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\binding.vcxproj.metaproj”(2) (默认目标)。
项目“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\binding.vcxproj.metaproj”(2)正在节点 1 上生成“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\src\libsass.vcxproj”(3) (默认目标)。
C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\Microsoft.Cpp.InvalidPlatform.Targets(23,7): error MSB8007: The Platform for project 'libsass.vcxproj' is invalid.  Platform='x64'. You may be seeing this message because you are trying to build a project without a solution file, and have specified a non-default Platform that doesn't exist for this project. [D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\src\libsass.vcxproj]
已完成生成项目“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\src\libsass.vcxproj”(默认目标)的操作 - 失败。
已完成生成项目“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\binding.vcxproj.metaproj”(默认目标)的操作 - 失败。
已完成生成项目“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\binding.sln”(默认目标)的操作 - 失败。

生成失败。

“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\binding.sln”(默认目标) (1) ->
“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\binding.vcxproj.metaproj”(默认目标) (2) ->
“D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\src\libsass.vcxproj”(默认目标) (3) ->
(InvalidPlatformError 目标) ->
  C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\Microsoft.Cpp.InvalidPlatform.Targets(23,7): error MSB8007: The Platform for project 'libsass.vcxproj' is invalid.  Platform='x64'. You may be seeing this message because you are trying to build a project without a solution file, and have specified a non-default Platform that doesn't exist for this project. [D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass\build\src\libsass.vcxproj]

    0 个警告
    1 个错误

已用时间 00:00:00.61
gyp ERR! build error
gyp ERR! stack Error: `C:\Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe` failed with exit code: 1
gyp ERR! stack     at ChildProcess.onExit (D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-gyp\lib\build.js:262:23)
gyp ERR! stack     at ChildProcess.emit (events.js:182:13)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:240:12)
gyp ERR! System Windows_NT 10.0.17134
gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "D:\\desktop\\Desktop\\vue-element-admin-proj\\vue-element-admin-master\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
gyp ERR! cwd D:\desktop\Desktop\vue-element-admin-proj\vue-element-admin-master\node_modules\node-sass
gyp ERR! node -v v10.15.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok
Build failed with error code: 1
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.11 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.11: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! node-sass@4.13.1 postinstall: `node scripts/build.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the node-sass@4.13.1 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\26305\AppData\Roaming\npm-cache\_logs\2020-01-19T12_13_19_200Z-debug.log
```

既然npm安装不了node-sass，那我们换cnpm是否可以
于是我就尝试安装cnpm，再用cnpm安装node-sass，命令如下：

```
$ npm install -g cnpm
$ cnpm install -g node-sass
```

竟然没有报错有没有！！！，于是我们思考一下，npm与cnpm的安装区别到底在哪里，是下载的源不一样，还是实现安装逻辑不一样？

我们给npm换源试一试？
我们都知道怎么给npm换taobao源，但是我们没有给sass单独设置过数据源，说做就做：

```
$ npm config set sass-binary-site http://npm.taobao.org/mirrors/node-sass
$ npm install node-sass
```

竟然也成功了！！！

上面的方法我是看一篇博客的，我tm当时也是傻x，一直在死磕`npm install --registry=https://registry.npm.taobao.org`，没想到单独给sass设置源


# 总结
官方不建议使用cnpm，说是会遇到诡异的bug，说的也是够瘆人的。
看github上大佬们都遇到了类似的问题，这样我心里就平衡了。
所以，如果你也遇到该问题，建议使用最后一种方法解决
```
$ npm config set sass-binary-site http://npm.taobao.org/mirrors/node-sass
$ npm install node-sass
```
