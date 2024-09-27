# Configuring Git

## Username

Query current username:

```sh
git config user.name
```

Globally set username:

```sh
git config --global user.name "John Smith"
```

## Email

Query user email:

```sh
git config user.email
```

Set user email:

```sh
git config --global user.email johnsmith@some-domain.com
```

## Set VSCode To Default Editor For Git

In order for this to work, need to make sure that the 'code' command is install in PATH, which can be done using the command palette in VSCode.

```sh
git config --global core.editor "code --wait"
```

Now when 'git commit' is called without the '-m' flage, the VSCode editor will pop up to provide a way to enter the commit message. Useful for writing long commit messages.

To see how to configure for other editors take a look at [Appendix C: Git Commands - Setup and Config](https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config)


