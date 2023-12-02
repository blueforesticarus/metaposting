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

<a href="#BACK" id="HERE">FIXED</a>

```nix
# shell.nix
{ pkgs ? import <nixpkgs> {} }:

pkgs.mkShell {
  packages = with pkgs; [
    pandoc
  ];
}
```

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

**wait** over <a href="#HERE">HERE</a> that nix-shell codeblock ~~isn't~~ wasn't rendering right


*** 
*Sat Dec  2 04:28:46 AM EST 2023*

### making mistakes

One of the benefits of markdown is that we don't have to worry so much about making mistakes.
That is because our mistakes have **locality**; errors (or changes) in one part of the document are unlikely to alter the output of other parts that are far away.
For an example of non-locality, consider rust, where the smallest change in one file will cause the entire output to change -- often from an executable program, to a lack of an executable program and a pile of error messages. ( tbf, on the set of inputs which compile, Rust *is* more local than, say, c -- meanwhile rust and something like js are local in *different ways* ). 

Locality is going to keep coming up, and [its hardly he only thing](https://en.wikipedia.org/wiki/Get_Used_to_It_(song)).

*anyway*

> `git show f4ec84c35a index.md`

```
~/blag god• ↑3 ❱ git show f4ec84c35a index.md
commit f4ec84c35abb2f5d4043c7cd925ac8e32efa7bdc (HEAD -> god)
Author: Erich Spaker <erichspaker@gmail.com>
Date:   Sat Dec 2 06:05:18 2023 -0500

    fix: I've mixed up my backticks and apostrophes again
```
```diff
diff --git a/index.md b/index.md
index f0ef522..2726240 100644
--- a/index.md
+++ b/index.md
@@ -58,12 +58,12 @@ Have a [nix-shell](https://nix.dev/tutorials/first-steps/declarative-shell.html

-'''nix
+```nix
 # shell.nix
 { pkgs ? import <nixpkgs> {} }:

 pkgs.mkShell {
   packages = with pkgs; [
     pandoc
   ];
 }
-'''
+```
```

<a href="#HERE" id="BACK">FIXED</a>

***

TIP: settings to get (imo) [optimal diff context](https://git-scm.com/docs/diff-config#Documentation/diff-config.txt-diffinterHunkContext):
```
~/blag god• ❱ git config --local diff.context 1
~/blag god• ❱ git config --local diff.interHunkContext 7
```

