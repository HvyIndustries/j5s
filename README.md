<h1 align="center"><code>j5s</code> - The painless Jenkins user CLI</h1>

<p align="center">
  <!--<img src="assets/preview.png" alt="j5s-logo" width="120px" height="120px"/>
  <br>-->
  <code>j5s</code> is a macOS terminal app written in Rust that lets you view your Jenkins build status<br>
    and logs with real-time data refreshing, directly from your terminal.
  <br>
</p>

<p align="center">
  <a href="https://j5s.hvy.io">Website</a>
  ·
  <a href="#quickstart">Quickstart</a>
  ·
  <a href="#documentation">Documentation</a>
  <br>
  <br>
</p>

<p align="center">
  <img
    width="600"
    src="https://raw.githubusercontent.com/HvyIndustries/j5s/master/assets/j5s-preview1.gif"
    alt="Demo of j5s running in the terminal"
  />
</p>

<hr>


## Quickstart

### Prerequisites

- A recent version of macOS on your machine (At this time `j5s` only supports macOS)
- A Jenkins instance running a recent version of Jenkins 

### Step 1 - Download & install j5s

Download the latest version of j5s from the [official releases page](https://github.com/HvyIndustries/j5s/releases) and unzip the file.

Open a terminal to the folder where you downloaded j5s and make the unzipped binary executable:
```bash
$ chmod +x j5s
```

Move the binary to your programs folder:
```
$ mv j5s /usr/local/bin
```

Test it works (Should output something like this: `j5s/0.2.0-beta`):
```
$ j5s version
```

### Step 2 - Connect to Jenkins

Run this command to start an interactive wizard:
```
$ j5s connect
```

* Set `jenkins_base_url` to the main hostname of your Jenkins instance without a trailing slash (eg. `https://jenkins.hvy.io`)
* Set `user` to your Jenkins username (eg. `john.stone`)
* Set `api_token`:
  * Open this URL in your browser `<jenkins_url>/me/configure` and create a new API token
  * Give the token a recognisable name like `j5s`, and copy the value
  * Set the value of the token as `api_token` (eg. `45678912345afe234feff23`)

The `connect` command will validate that your Jenkins base URL is correct and that your credentials work.

Once completed, you're ready to start using j5s! Read on to learn more about the [available commands](#commands-overview)


## Documentation

### Overview / Concepts

`j5s` uses standard Jenkins APIs to fetch build status information. It should work with any version of Jenkins and does not require any special plugins to be installed.


### Commands Overview

* [`status`](#command-status) - Show build status
* [`logs`](#command-logs) - Show build logs
* [`connect`](#command-connect) - Wizard to assist with first-time connection to Jenkins instance
* [`version`](#command-version) - Display j5s version


### Command Details

#### Command: `status`

Usage
```
$ j5s status <search> [build_num] [-f]
```

Searches for a Jenkins job matching `<search>` and displays its status. If `-f` is passed, the status will auto-refresh every ~5 seconds.

You can provide `[build_num]` to see the status for a specific build of the matched job. If not provided, the most most recent build for the job will be used instead.

This is quite useful if you are working on a branch with a Jenkins multibranch pipeline - you can run `j5s status <branch> -f` and it will auto-update to display the most recent build whenever you push new commits.

If the Jenkins job is a declarative pipeline, the pipeline steps and their status will also be displayed.

> **Search tip:** If you are searching for a job that exists in multiple folders or multibranch pipelines (such as `main`) you can make your search more specific by adding the name of the parent and wrapping it in quotes. For example `j5s status "ui main"` instead of `j5s status main`.
>
> If multiple jobs match your search, j5s will show you up to 10 matching jobs to choose from. If there are more than 10 matches, you need to make your search more specific.


#### Command: `logs`

Usage
```
$ j5s logs <search> [build_num] [-f]
```

Searches for a Jenkins job matching `<search>` and displays all available logs. If the build is running and `-f` is passed, the logs will update every ~5 seconds until the build finishes.

You can provide `[build_num]` to see the logs for a specific build of the matched job. If not provided, the most most recent build for the job will be used instead.


#### Command: `connect`

Usage:
```
$ j5s connect
```

Starts an interactive wizard to help you connect to a Jenkins instance. Generally you only ever need to do this when first connecting to Jenkins, or if your username or API token have changed.

Three fields are required:
* `jenkins_base_url` -> the main hostname of your Jenkins instance without a trailing slash (eg. `https://jenkins.hvy.io`)
* `user` -> your Jenkins username (eg. `john.stone`)
* `api_token` -> your Jenkins API token. To create one:
  * Open this URL in your browser `<jenkins_url>/me/configure` and create a new API token
  * Give the token a recognisable name like `j5s`, and copy the value
  * Set the value of the token as `api_token` (eg. `45678912345afe234feff23`)


#### Command: `version`

Usage:
```
$ j5s version
```

Prints the installed version of `j5s`.


## Troubleshooting

Something not working? Please [make a bug report](https://github.com/HvyIndustries/j5s/issues/new/choose) so that I can fix it.

### Common problems

todo


## Where is the source code?

j5s is not an open source project. This repository is for documentation and to be a public forum for users to request features and report bugs.

