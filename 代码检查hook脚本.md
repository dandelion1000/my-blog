```
https://dev.tencent.com/u/wangyuzju/p/saas-mis-fe/git/blob/master/package.json
```

![image-20190412105245706](/Users/zhouxianxian/Library/Application Support/typora-user-images/image-20190412105245706.png)

```
https://dev.tencent.com/u/wangyuzju/p/saas-mis-fe/git/blob/master/.eslintrc.js

参考eslint的规则就行

https://dev.tencent.com/u/wangyuzju/p/saas-mis-fe/git/blob/master/.eslintignore   三方的库，可以在这里配置忽略
```

```
https://segmentfault.com/a/1190000009546913
```

`pre-commit` 钩子在键入提交信息前运行。 它用于检查即将提交的快照，例如，检查是否有所遗漏，确保测试运行，以及核查代码。 如果该钩子以非零值退出，Git 将放弃此次提交，不过你可以用 `git commit --no-verify` 来绕过这个环节。 你可以利用该钩子，来检查代码风格是否一致（运行类似 `lint` 的程序）、尾随空白字符是否存在（自带的钩子就是这么做的），或新方法的文档是否适当。


如果把 Lint 挪到本地，并且每次提交只检查本次提交所修改的文件，上面的痛点就都解决了。Feedly 的工程师 [Andrey Okonetchnikov](https://www.npmjs.com/~okonet) 开发的 [lint-staged](https://github.com/okonet/lint-staged) 就是基于这个想法，其中 staged 是 Git 里面的概念，指待提交区，使用 `git commit -a`，或者先 `git add` 然后 `git commit` 的时候，你的修改代码都会经过待提交区。

```
npm install -D lint-staged
yarn add --dev lint-staged
```

    "gitHooks": {
        "pre-commit": "lint-staged"
    },
    "lint-staged": {
        "*.js": [
            "vue-cli-service lint",
            "git add"
        ],
        "*.vue": [
            "vue-cli-service lint",
            "git add"
        ]
    }



![图片](https://coding-net-production-pp-ci.codehub.cn/2061b64c-f258-4b0f-bc4a-677ac4f078ab.png)

系统提示找到错误36 errors found.34 errors potentially fixable with the `--fix` option.

执行package.json中lint的配置脚本：

![图片](https://coding-net-production-pp-ci.codehub.cn/43713c64-abb1-43a7-b72d-f823b2686dbe.png)



在项目目录执行 yarn lint，会自动检查项目整体错误，并修复部分error和warning，无法自动修复的需要手动去改。