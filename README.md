# git-standup

> Recall what you did on the last working day ..or be nosy and find what someone else did.

## Install

You can install `git-standup` using one of the options listed below

| Source | Command |
| --- | --- |
| curl | `curl -L https://raw.githubusercontent.com/kamranahmedse/git-standup/master/installer.sh | sudo sh` |
| npm | `npm install -g git-standup` |
| brew | `brew update && brew install git-standup` |
| aur | `pacaur -S git-standup-git` |
| manual | Clone and run `make install` |  

### Usage

```shell
git standup [-a <author name>] 
            [-w <weekstart-weekend>]
            [-m <max-dir-depth>]
            [-d <since-days-ago>] 
            [-u <until-days-ago>]
            [-D <date-format>]
            [-L]
            [-g] 
            [-h] 
            [-f]
            [-s]
```
### Options

Running `git standup` by default gives you the standup report for the last working day considering the week days are from Monday to Friday. And if you run it in a git repository it will give you the standup report for that otherwise, it will look one step down and give you the report for all the repositories found in the current directory. 

You can modify how `git standup` behaves using the options below

| Option | Description |
| --- | --- |
| a | Specify author to restrict search to e.g. `-a "Kamran Ahmed"` or `-a "all"` |
| w | Specify week start and end days e.g. in UAE weekdays are from Sunday to Thursday so you can do `-w SUN-THU`|
| m | Specify the depth of recursive directory search e.g. `-m 3` defaults to two |
| d | Specify the number of days back to include e.g. `-d 30` to get for a month |
| u | Specify the number of days back till which standup should run e.g. `-u 3` |
| L | Toggle inclusion of symbolic links in recursive directory search |
| D | Specify the date format for "git log" (default: relative) [possible values](https://git-scm.com/docs/git-log#git-log---dateltformatgt) |
| h | Display the help screen |
| g | Show if commit is GPG signed (G) or not (N) |
| f | Fetch the latest commits beforehand |
| s | Silences the no activity message (useful when running in a directory having many repositories) |

Examples:
  git standup -a "John Doe" -w "MON-FRI" -m 3
```

For the basic usage, all you have to do is run `git standup` in a repository or a folder containing multiple repositories

## Single Repository Usage

To check all your personal commits from last working day, head to the project repository and run

```shell
$ git standup
```

![git standup](http://i.imgur.com/wyo4s9E.gif)

## Multiple Repository Usage
Open a directory having multiple repositories and run

```shell
$ git standup
```

![git standup](http://i.imgur.com/4xmkA49.gif)

This will show you all your commits since the last working day in all the repositories inside. 

## Directory depth

By default the script searches only in the current directory or one
level deep. If you want to increase that, use the `-m` switch.

```shell
$ git standup -m 3
```

### Directory whitelisting

If you want to restrict the standup to some paths, you can whitelist them by adding them to a `.git-standup-whitelist` file. For example if you have the below directory structure

    ├── Workspace              # All your projects are here
    │   ├── project-a          # Some git repository called project-a
    │   ├── project-b          # Some git repository called project-b
    │   ├── sketch-files       # Some sketch files
    │   ├── mockups            # Some balsamiq mockups
    │   └── ...                # etc.
    └── ...

And you want the `git-standup` to show logs for only `project-a` and `project-b`, you can do that by creating a `.git-standup-whitelist` file under the `Workspace` directory with the below contents and it will only consider these directories for the standup

```
project-a
project-b
```

## Checking someone else's commits

If you want to find out someone else's commits do

```shell
# Considering their name on git is "John Doe"
$ git standup -a "John Doe"
```
![git standup](http://i.imgur.com/sYICxW8.gif)

## Check what every contributor did

If you want to find out someone else's commits do

```shell
$ git standup -a "all"
```

## Commits from `n` days ago

If you would like to show all your/someone else's commits from n days ago, you can do

```shell
# Show all my commits from 4 days ago
$ git standup -d 4

# Show all John Doe's commits from 5 days ago
$ git standup -a "John Doe" -d 5
```

![git standup -d 5](http://i.imgur.com/j7Ma760.gif)

## [Identifying Signed Commits](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work)

Add `-g` flag to check the GPG info

```shell
$ git standup -g
```

![GPG Info](http://i.imgur.com/bwJzPft.gif)

## Specifying the date format

Add `-D` flag to specify the date format. Default is `relative`

Please note that it accepts the same format that you could pass while doing git log. For example

```shell
$ git standup -D relative
# Or instead of relative, it could be local|default|iso|iso-strict|rfc|short|raw etc
```

## Changing the Weekdays

By default, it considers that the work week starts on Monday and ends on Friday. So if you are running this on any day between Tuesday and Friday, it will show you your commits from the last day. However, if you are running this on Monday, it will show you all your commits since Friday.

If you want to change this, like I want because here in Dubai working days are normally Sunday to Thursday, you will have to do the following

```shell
$ git standup -w "SUN-THU"
```

## Fetch commits before showing standup

If you have many repositories that you want to generate a standup for, it may be useful to automatically run `git fetch` before viewing the standup.

If you would like to automatically run `git fetch --all` before printing the standup, you can add the `-f` flag, as show below

```shell
$ git standup -f
```

## Mixing options

Of course you can mix the options together but please note that if you provide the number of days, it will override the weekdays configuration (`MON-FRI`) and will show you the commits specifically from `n` days ago.

```shell
# Show all the John Doe's commits from 5 days ago
$ git standup -a "John Doe" -d 5
```

## Motivation

We have daily standup meetings at our workplace and I was used to checking the heat map on my Github profile or running git log (one by one in each of the projects) to note what I did and it seemed to be a bit cumbersome. To automate it, I searched and came across some of [the](http://code.joejag.com/2013/everyday-git-aliases.html) [aliases](https://coderwall.com/p/f4shwg/git-for-daily-standup) [and](https://gist.github.com/pathikrit/fb75ba009960c4ed9ddf) [snippets](https://github.com/stephenmathieson/git-standup) that people had been using but none of them directly served my purpose and so I spent a little time over the weekend to write this utility.


## License

MIT © [Kamran Ahmed](http://kamranahmed.info)
