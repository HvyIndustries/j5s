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

### Step 2 - Configure j5s

Run this command to start an interactive wizard to assist with setup:
```
$ j5s configure
```

* Set `jenkins_base_url` to the main hostname of your Jenkins instance without a trailing slash (eg. `https://jenkins.hvy.io`)
* Set `user` to your Jenkins username (eg. `john.stone`)
* Set `api_token`:
  * Open this URL in your browser `<jenkins_url>/me/configure` and create a new API token
  * Give the token a recognisable name like `j5s`, and copy the value
  * Set the value of the token as `api_token` (eg. `45678912345afe234feff23`)

The `configure` command will validate that your Jenkins base URL is correct and that your credentials work.

Once completed, you're ready to start using j5s! Read on to learn more about the [available commands](#commands-overview)


## Documentation

### Overview / Concepts

`j5s` uses standard Jenkins APIs to fetch build status information. It should work with any version of Jenkins and does not require any special plugins to be installed.

If you can access the Jenkins web UI, `j5s` will probably work for you.


## Commands

* [`status`](#command-status) - Show the status of a specific job/build
* [`logs`](#command-logs) - Show logs for a specific job/build
* [`list-jobs`](#command-list-jobs) - List all jobs defined in Jenkins
* [`list-builds`](#command-list-builds) - List up to 50 last builds of a specific job
* [`configure`](#command-configure) - Set-up j5s configuration (first-time setup)
* [`version`](#command-version) - Display j5s version


### Command: `status`

**Usage**
```
$ j5s status <search> [build_num] [-f]
```

Searches for a Jenkins job matching `<search>` and displays its status. If `-f` is passed, the status will auto-refresh every ~5 seconds.

You can provide `[build_num]` to see the status for a specific build of the matched job. If not provided, the most most recent build for the job will be used instead.

This is quite useful if you are working on a branch with a Jenkins multibranch pipeline - you can run `j5s status <branch> -f` and it will auto-update to display the most recent build whenever you push new commits.

If the Jenkins job is a declarative pipeline, the pipeline steps and their status will also be displayed.

> **Search tip:** If you are searching for a job that exists in multiple folders or multibranch pipelines (such as `main`) you can make your search more specific by adding the name of the parent at the start. For example `j5s status ui main` instead of `j5s status main`.
>
> If multiple jobs match your search, j5s will show you up to 10 matching jobs to choose from. If there are more than 10 matches, you need to make your search more specific.

**Examples:**
* `j5s status jobitem` -> Display the status for the last build of a job with a name matching `jobitem`
* `j5s status jobitem -f` -> Same as previous, except that the status will be updated every ~5 seconds. If a new build starts, it will be displayed automatically
* `j5s status jobitem 5` -> Displays status for build #5 of a job with a name matching `jobitem`
* `j5s status jobitem 5 -f` -> Save as previous, except that the status will be updated every ~5 seconds
* `j5s status myfolder jobitem 5` -> Display the status for build #5 of a job with a name matching `jobitem` within the folder `myfolder`


### Command: `logs`

**Usage**
```
$ j5s logs <search> [-f]
```

Searches for a Jenkins job matching `<search>` and displays all available logs. If the build is running and `-f` is passed, the logs will update every ~5 seconds until the build finishes.

You can provide a build number at the end of a search to see the logs for a specific build of the matched job. If not provided, the most most recent build for the job will be used instead.

**Examples:**
* `j5s logs jobitem` -> Display logs for the last build of a job with a name matching `jobitem`
* `j5s logs jobitem -f` -> Same as previous, except if the build is running logs will be tailed (new lines appended evert ~5 seconds)
* `j5s logs jobitem 5` -> Displays logs for build #5 of a job with a name matching `jobitem`
* `j5s logs myfolder jobitem 5` -> Displays logs for build #5 of a job with a name matching `jobitem` within the folder `myfolder`


### Command: `list-jobs`

**Usage**
```
$ j5s list-jobs [search]
```

If no `[search]` is provided, list all of the top-level configured jobs, folders, etc in Jenkins. If `[search]` is provided, list of all of the configured jobs, folders, etc. within the search.

**Examples:**
* `j5s list-jobs` -> List all top-level jobs
* `j5s list-jobs myfolder` -> List all jobs inside the `myfolder` folder (or multibranch pipeline)


### Command: `list-builds`

**Usage**
```
$ j5s list-builds <search> [-f]
```

Lists the last 50 builds for a job that matches `<search>`. If `-f` is passed, the list will auto-update every ~5 seconds.

**Examples:**
* `j5s list-builds jobitem` -> List up to 50 last builds for the `jobitem` job
* `j5s list-builds myfolder jobitem` -> List up to 50 last builds for the `jobitem` job inside the `myfolder` folder
* `j5s list-builds jobitem -f` -> List up to 50 last builds for the `jobitem` job, auto-update the list every ~5 seconds.


### Command: `configure`

**Usage**
```
$ j5s configure
```

Starts an interactive wizard to help you connect to a Jenkins instance. **Generally you only ever need to do this when first connecting to Jenkins, or if your username or API token have changed.**

Three fields are required:
* `jenkins_base_url` -> the main hostname of your Jenkins instance without a trailing slash (eg. `https://jenkins.hvy.io`)
* `user` -> your Jenkins username (eg. `john.stone`)
* `api_token` -> your Jenkins API token. To create one:
  * Open this URL in your browser `<jenkins_url>/me/configure` and create a new API token
  * Give the token a recognisable name like `j5s`, and copy the value
  * Set the value of the token as `api_token` (eg. `45678912345afe234feff23`)


### Command: `version`

**Usage**
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

