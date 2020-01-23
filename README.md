# dENVer

Treat your secrets like password

## purpose

We live in a devops world, the devops world is awesome, so many neat tools and 
deployment management, API, ...

Many vendors will treat the security as "next vendor's problem".

As a user, we end-up with many dotfiles containing critical API keys in 
plain-text, or we just `export BASH_VARS=SeCrEt`, and the secret end-up in 
`.bash_history`.

On another side, we start to implement saner password management policies, 
thanks to tools such as `password-store`, `keepass`, `1password`, and so many 
more.

Let's try to fix it.

Denver is super simple and tool-agnostic script which let you export the 
environment variables AWS and Vault love so much, from your password manager 
(granted it offers you a way to write it to `stdout`). And set an alias 
(`fdenver`) to forget about it when you're done (or just close the terminal, 
I'm not your boss)

## installation

It's currently a WIP (work in progress) so it won't be pushed to pypi (yet), 
but due to being a pretty dumb wrapper around more mature tools, you can start 
to use it safely with actual secrets.

It could work, with some effort, in windows, but it's out of scope for now.

The `demo.cfg` file can be copied to `$HOME/.denver.cfg` 

```bash
pip install -r requirements.txt
ln -s `pwd`/denver.py $HOME/.local/bin
```

## usage

### on the password manager side

Store a secret in the form of:

```
VAR_NAME_FOO=a_secret
VAR_BAR=another_secret
```

If you use keepass, use the `Notes` field.

### on denver side

Adapt the command to your use-case, examples are provided for `keepassxc` and 
`gopass`

It should work without any problem in any shell providing subshell support (ie. 
bash and zsh)

If your environment already has a variable with the same name in its scope, 
`denver` won't overwrite it, nor set it to be unset. 

```bash
source <(denver.py -n NAME)

# look at the variables being correctly set up:
env

# forget about these
fdenver
```

### more help

Haven't you tried this already ?

```bash
denver.py --help
```

### a note about stdin password prompts

You can't (easily) reach the subshell's stdin, it means you should use an 
external prompt program if the password tool give an interactive prompt, use a 
graphical tool in order to pipe your password there (cf. `keepassxc` command)
