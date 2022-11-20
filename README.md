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
    width="400"
    src="https://raw.githubusercontent.com/HvyIndustries/j5s/master/assets/preview.gif"
    alt="Demo of j5s running in the terminal"
  />
  <br>
  <sub><sup><code>j5s</code> is proprietary, closed-source software.<sub><sup>
</p>

<hr>


## Quickstart

### Prerequisites

- A recent version of macOS on your machine (At this time `j5s` only supports macOS)
- A Jenkins instance running a recent version of Jenkins 

### Step 1 - Download & install j5s

Download the latest version of j5s from the [official releases page](https://github.com/HvyIndustries/j5s/releases).

Open a terminal to the folder where you downloaded j5s and make the binary executable:
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

Create a j5s config file in your home directory:
```
$ mkdirp ~/.config/
$ touch ~/.config/j5s.toml
```

Open `~/.config/j5s.toml` with your favourite editor and paste this inside:
```
jenkins_base_url = ""
user = ""
api_token = ""
```

* Set `jenkins_base_url` to the main hostname of your Jenkins instance without a trailing slash (eg. `https://jenkins.hvy.io`)
* Set `user` to your Jenkins username (eg. `john.stone`)
* Set `api_token`:
  * Open this URL in your browser `<jenkins_url>/me/configure` and create a new API token
  * Give the token a recognisable name like `j5s`, and copy the value
  * Set the value of the token as `api_token` (eg. `45678912345afe234feff23`)

> *NOTE: In future the connect step will be simplified by the addition of a `connect` command that will create the config file for you.*

That's it, you're ready to start using `j5s`!

For example:
```
$ j5s status my-job-name
```

[Learn more about the available commands](#commands)


## Documentation

### Overview / Concepts

`j5s` uses standard Jenkins APIs to fetch build status information. It should work with any version of Jenkins and does not require any special plugins to be installed.


### Commands

#### `status`

Usage
```
$ j5s status <search> [build_num] [-f]
```

Searches for a Jenkins job matching `<search>` and displays its status. If `-f` is passed, the status will auto-refresh every ~5 seconds.

You can provide `[build_num]` to see the status for a specific build of the matched job. If not provided, the most most recent build for the job will be used instead.

This is quite useful if you are working on a branch with a Jenkins multibranch pipeline - you can run `j5s status <branch> -f` and it will auto-update to display the most recent build whenever you push new commits.

If the Jenkins job is a declarative pipeline, the pipeline steps and their status will also be displayed.


#### `logs`

Usage
```
$ j5s logs <search> [build_num] [-f]
```

Searches for a Jenkins job matching `<search>` and displays all available logs. If the build is running and `-f` is passed, the logs will update every ~5 seconds until the build finishes.

You can provide `[build_num]` to see the logs for a specific build of the matched job. If not provided, the most most recent build for the job will be used instead.


#### `version`

Usage:
```
$ j5s version
```

Prints the installed version of `j5s`.


### Troubleshooting

Something not working? Please [make a bug report](https://github.com/HvyIndustries/j5s/issues/choose) so that I can fix it.

todo

