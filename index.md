<style>
  a:target {
    background-color: yellow;
  }
</style>

# day 0
*Fri Dec  1 02:10:23 PM EST 2023*

## getting started (again)

Some things never change

> `mkdir blag ; cd blag ; git blag`

```
~ ❱ mkdir blag ; cd blag ; git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint: 	git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint: 	git branch -m <name>
Initialized empty Git repository in /home/user/blag/.git/
~/blag master• ❱ 
```

👀 or not...

```
~/blag ❱ git config --global init.defaultBranch somethingterriblyoffensive
~/blag ❱ rm -rf .git
~/blag ❱ git init
Initialized empty Git repository in /home/user/blag/.git/
~/blag somethingterriblyoffensive• ❱
```

Hmm... better to keep it simple.

```
git branch -m god
~/blag god ❱ 
```

## dependencies

Have a [nix-shell](https://nix.dev/tutorials/first-steps/declarative-shell.html "nix-shell is part of Nix: the reproducible, declarative, reliable package manager.").

> `vim shell.nix`

<a href="#BACK" id="HERE">???</a>

'''nix
# shell.nix
{ pkgs ? import <nixpkgs> {} }:

pkgs.mkShell {
  packages = with pkgs; [
    pandoc
  ];
}
'''

Reproducible build environment as [WOMB](## "works on my build") point of truth: [(install nix)](https://nixos.org/download).

> `nix-shell --pure`

```
~/blag god• 7m 6.1s ❱ nix-shell --pure
[nix-shell:~/blag]$ head -10 index.md
```
```md
# day 0
*Fri Dec  1 02:10:23 PM EST 2023*

## getting started (again)

Some things never change

> `mkdir blag ; cd blag ; git blag`
```
```
[nix-shell:~/blag]$ pandoc index.md | head -8`
```
```html
<h1 id="day-0">day 0</h1>
<p><strong>Fri Dec 1 02:10:23 PM EST 2023</strong></p>
<h2 id="getting-started-again">getting started (again)</h2>
<p>Some things never change</p>
<blockquote>
<p><code>mkdir blag ; cd blag ; git blag</code></p>
</blockquote>
<hr />
```

Now do the unix thing

> `[nix-shell:~/blag]$ pandoc index.md > index.html`

and then

> `brave index.html # pull that shit up`{.bash}

*** 

### and of course
```
~/blag god• ❱ git add index.html index.md  shell.nix
~/blag god• ❱ git commit -m "let there be light" # --amend will come in handy
[god e1a6ae6] let there be light
 Date: Sat Dec 2 04:45:44 2023 -0500
 3 files changed, 201 insertions(+)
 create mode 100644 index.html
 create mode 100644 index.md
 create mode 100644 shell.nix
```

***
*Sat Dec  2 04:28:46 AM EST 2023*

## ~~learning~~ markdown (again)

```md
~/blag god• ❱ rg "the thing about learning markdown is" -A 1 --heading | tail -1
**you are already writing it**
```

Everything looking good so far.

**wait** over <a href="#HERE" id="BACK">HERE</a> that nix-shell codeblock isn't rendering.