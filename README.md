<p align="center">
  <a href="https://devenv.sh">
    <picture>
      <source media="(prefers-color-scheme: light)" srcset="logos/devenv-horizontal-light-bg.svg">
      <source media="(prefers-color-scheme: dark)" srcset="logos/devenv-horizontal-dark-bg.svg">
      <img src="logos/devenv-horizontal-light-bg.svg" width="500px" alt="devenv logo">
    </picture>
  </a>
</p>

# FORKED VERSION

This fork removes all functionality related to uploading data. You are welcome to use this fork if you like, it will
result in a relatively long first compile of the devenv rust code. I make no promises with respect to maintenance or
warranty, I maintain this primarily for my own purposes and when I have migrated off of devenv (which you should do as
well, given the disgraceful behavior of it's maintainers adding this useless, borderline malicious[1] functionality in the
first place) I will archive this repository.

Thanks for playing, good luck avoiding the enshittification of everything else on the internet.

---


[1] To be clear, the original version of this code added functionality that:

1. tarred up every file in your repository via `git ls-files`.
2. Sent it to an unknown server, ostensibly owned by the devenv people (cachix, I suppose?)
3. Fed it to an AI.

If you set DO_NOT_TRACK, it would simply tag the request with a 'don't use this for telemetry' flag and _still send it
to the third party._

It shouldn't take you more than 2 minutes to understand why that's profoundly stupid in the least, and so we're left
with two options.

1. The original maintainers are so profoundly braindead stupid that they would not once think about what the
   implications of the decision to send all of your code to a third party would be.
2. They knew, and didn't care.

In the case of the first, then you probably shouldn't use code written by such profound idiots.
If the latter, then you probably shouldn't use code written by such profound assholes.

In either case, you should probably migrate off of devenv as soon as possible. You should, further, not trust me either,
but fortunately for you I don't run any telemetry servers and you can look at the top two commits of this repo.

To the original authors, shame on you.

# [devenv.sh](https://devenv.sh) - Fast, Declarative, Reproducible, and Composable Developer Environments

[![Built with Nix](https://img.shields.io/static/v1?logo=nixos&logoColor=white&label=&message=Built%20with%20Nix&color=41439a)](https://builtwithnix.org)
[![Discord channel](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fdiscord.com%2Fapi%2Finvites%2FnaMgvexb6q%3Fwith_counts%3Dtrue&query=%24.approximate_member_count&logo=discord&logoColor=white&label=Discord%20users&color=green&style=flat)](https://discord.gg/naMgvexb6q)
![License: Apache 2.0](https://img.shields.io/github/license/cachix/devenv)
[![Version](https://img.shields.io/github/v/release/cachix/devenv?color=green&label=version&sort=semver)](https://github.com/cachix/devenv/releases)
[![CI](https://github.com/cachix/devenv/actions/workflows/buildtest.yml/badge.svg)](https://github.com/cachix/devenv/actions/workflows/buildtest.yml?branch=main)

Running ``devenv init`` generates ``devenv.nix``:

```nix
{ pkgs, ... }:

{
  # https://devenv.sh/basics/
  env.GREET = "devenv";

  # https://devenv.sh/packages/
  packages = [ pkgs.git ];

  enterShell = ''
    hello
  '';

  # https://devenv.sh/tests/
  enterTest = ''
    echo "Running tests"
    git --version | grep --color=auto "${pkgs.git.version}"
  '';

  # https://devenv.sh/languages/
  languages.nix.enable = true;

  # https://devenv.sh/scripts/
  scripts.hello.exec = "echo hello from $GREET";

  # https://devenv.sh/services/
  services.postgres.enable = true;

  # https://devenv.sh/git-hooks/
  git-hooks.hooks.shellcheck.enable = true;

  # https://devenv.sh/processes/
  processes.ping.exec = "ping localhost";
}

```

And ``devenv shell`` activates the environment.

## Commands

```
$ devenv
https://devenv.sh 1.4.0: Fast, Declarative, Reproducible, and Composable Developer Environments

Usage: devenv [OPTIONS] [COMMAND]

Commands:
  init       Scaffold devenv.yaml, devenv.nix, .gitignore and .envrc.
  generate   Generate devenv.yaml and devenv.nix using AI
  shell      Activate the developer environment. https://devenv.sh/basics/
  update     Update devenv.lock from devenv.yaml inputs. http://devenv.sh/inputs/
  search     Search for packages and options in nixpkgs. https://devenv.sh/packages/#searching-for-a-file
  info       Print information about this developer environment.
  up         Start processes in the foreground. https://devenv.sh/processes/
  processes  Start or stop processes. https://devenv.sh/processes/
  tasks      Run tasks. https://devenv.sh/tasks/
  test       Run tests. http://devenv.sh/tests/
  container  Build, copy, or run a container. https://devenv.sh/containers/
  inputs     Add an input to devenv.yaml. https://devenv.sh/inputs/
  repl       Launch an interactive environment for inspecting the devenv configuration.
  gc         Delete previous shell generations. See https://devenv.sh/garbage-collection
  build      Build any attribute in devenv.nix.
  direnvrc   Print a direnvrc that adds devenv support to direnv. See https://devenv.sh/automatic-shell-activation.
  version    Print the version of devenv.
  help       Print this message or the help of the given subcommand(s)

Options:
  -V, --version
          Print version information
  -v, --verbose
          Enable additional debug logs.
  -q, --quiet
          Silence all logs
      --log-format <LOG_FORMAT>
          Configure the output format of the logs. [default: cli] [possible values: cli, tracing-full]
  -j, --max-jobs <MAX_JOBS>
          Maximum number of Nix builds at any time. [default: 5]
  -u, --cores <CORES>
          Maximum number CPU cores being used by a single build. [default: 2]
  -s, --system <SYSTEM>
          [default: aarch64-darwin]
  -i, --impure
          Relax the hermeticity of the environment.
      --eval-cache
          Cache the results of Nix evaluation.
      --refresh-eval-cache
          Force a refresh of the Nix evaluation cache.
      --offline
          Disable substituters and consider all previously downloaded files up-to-date.
  -c, --clean [<CLEAN>...]
          Ignore existing environment variables when entering the shell. Pass a list of comma-separated environment variables to let through.
      --nix-debugger
          Enter the Nix debugger on failure.
  -n, --nix-option <NIX_OPTION> <NIX_OPTION>
          Pass additional options to nix commands, see `man nix.conf` for full list.
  -o, --override-input <OVERRIDE_INPUT> <OVERRIDE_INPUT>
          Override inputs in devenv.yaml.
  -h, --help
          Print help (see more with '--help')
```

## Documentation

- [Getting Started](https://devenv.sh/getting-started/)
- [Basics](https://devenv.sh/basics/)
- [Roadmap](https://devenv.sh/roadmap/)
- [Blog](https://devenv.sh/blog/)
- [`devenv.yaml` reference](https://devenv.sh/reference/yaml-options/)
- [`devenv.nix` reference](https://devenv.sh/reference/options/)
- [Contributing](https://devenv.sh/community/contributing/)
