# git 以及 github

## git 配置

以类似于工作区的形式，对应不同的 git 配置

git 配置同样有三（四）个等级：

1. 全系统级别：`$(prefix/etc/gitconfig)`

    （`/etc/gitconfig`）（Windows 下在 Git 安装目录中）
    对应 git config --system

2. 用户级别: `$XDG_CONFIG_HOME/git/config`

    当 XDG_CONFIG_HOME 没有设置或者为空时（`$HOME/.gitconfig`）
    对应 git config --global

3. 工作区级别（`.git/config`）

    对应 git config --local

4. `$GIT_DIR/config.worktree`

    当 `$GIT_DIR/config` 中存在 `extensions.worktreeconfig` 才会搜索

### 配置文件语法（Syntax）

节（section） + 变量(variable)

```txt
[<section>] # 不区分大小写，a-zA-Z0-9-
    <v> = <v> #变量名不区分大小写，a-zA-Z0-9.-
[<section> "<subsection>"] #子节区分大小写，除了("" 以及 换行)
```

### 包括（Include）

使用 `include` 和 `includeIf` 允许包含其他源的配置，如果为真会立刻插入（git 读取 config 时，有重复值会保留最后一次的值作为结果）

```txt
# kerword:<data depend on kerword>
[includeIf "gitdir:/path/to/]
    path = <xxxx>
# gitdir 关键字后面的数据用作 glob 模式，`.git` 目录位置与模式匹配，满足包含条件
```

- 支持 `~/`,`./`,`/`。
- 当 `xxx` 会匹配 `**/xxx`
- 当 `/xxx/` 会匹配 `/xxx/**`

Include 中还有一些其他关键字

- `gitdir/i`（在大小写不敏感的文件系统上）
- `onbranch`
- `hasconfig:remote.*.url`

### 我的用处

目前用来配置不同的 `user.name` 以及 `user.email`。

## github 本地多用户

对于 github 如何实现本地多个账户灵活切换（只是想有一点点不被发现的小乐趣🕶️

### 实现方式(仅仅使用了 https 协议)

#### Github Cli

笨笨的喵只想使用现成的工具，那么 Github CLI 自然拿来就用

> Githubcli 官网地址：<https://cli.github.com/>

1. Authenticate gh and git with GitHub

    ```shell
    gh auth login
    ```

2. 再次 login other user

3. 切换用户

    ```shell
    gh auth switch
    ```

4. 还需要配置对应的 git 设置

```shell
gh auth setup-git
```

> `switch` 切换的是 gh 中 github 的账户，`setup-git` 使用 gh 作为 git 的 credential helper

- `gh:github.com:`
- `gh:github.com:<user-name>`
- `git:https://github.com`

#### 探索一种可能的实现方式

1. 下载 [Git Credential Manager](https://github.com/GitCredentialManager/git-credential-manager)

2. 确保 `git confg credential.helper` 结果是 `manager`

3. 使用 `git credential-manager github login` 添加新的 account

    - `git:https://<username>@github.com`

4. clone 的时候使用 `https://<username>@github.com/<username>/<repo>`

喵喵正在探索中
