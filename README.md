# subgit

You have a git repository, and the files are ignored. But maybe those ignored files are really important, and you want to manage them with git?

**subgit** is a tool that helps you manage git repositories within a git repository. Not a submodule of git but a separate git repo that exists inside the current git repo. Completely separate, only sharing working directory.

Let's say your repo is like this:

```
project-x/
├── index.js
├── .env             <--- git ignored
└── .gitignore
```

Using **subgit**, you will create a bare git repo named `subgit` in the `.git` folder. You can use this repo to do anything, such as managing ignored files (`.env`, `docs/`, `.dev/`,...).

```
project-x/
├── .git/
│   └── subgit/
├── index.js
├── .env
└── .gitignore
```

`.env` file ignored by `.gitignore` in current git repo, but tracking by `.git/subgit` repo.

## Usage

```
subgit [command]

Avaiable commands:
  init              Initialize a subgit repo

  list              List all subgit repos. Alias: `ls`

  remove            Remove a subgit repo, before remove will show confirm, type 'yes' to remove. Alias: `rm`

  shell             Enter subshell of subgit, this subshell already define GIT_DIR and GIT_WORK_TREE.
                    You can run commands like `git status`, `git commit`, `lazygit`,.. etc.

  lazygit           run lazygit on subgit. This is a shortcut for `subgit shell` then run `lazygit`. Alias: `lazy`

  [git command]     Other commands of git, like: status, branch, checkout, reset, push,... etc
```

## Environment variables

| Name                 | Desc                                                                                                     |
| -------------------- | -------------------------------------------------------------------------------------------------------- |
| `SUBGIT`             | Name of subgit folder. If specified then target folder will be ".git/subgit-$SUBGIT", else ".git/subgit" |
| `IS_SUBGIT_SUBSHELL` | **subgit** use this env to indicated that current shell is in a subgit repo                              |

## Examples

### Create a subgit repo

```sh
subgit init
```

### Create a subgit repo with custom name

```sh
SUBGIT=foo subgit init
```

### List current subgit repos

```sh
subgit list

# or:
subgit ls
```

### Remove a subgit repo

#### Remove default repo

```sh
subgit remove
```

#### Remove custom repo

```sh
SUBGIT=foo subgit remove
```

### Open a subgit repo with Lazygit

```sh
subgit lazygit

# or
subgit lazy

# or
SUBGIT=foo subgit lazy
```

### Open a subshell in context of subgit repo

```sh
subgit shell

# or
SUBGIT=foo subgit shell
```

### Run git commands

```sh
subgit status
subgit add [file]
subgit add [file] --force
subgit commit
subgit push

# or
SUBGIT=foo git add status
```
