# Blocking Hashtags from your Mastodon instance

> [!NOTE]
> This repository has moved. For the latest version see [code.thms.uk/michael/mastodon_block_hashtags](https://code.thms.uk/michael/mastodon_block_hashtags)

This repository contains a script for blocking / removing posts containing media with certain hash-tags and blocking their authors.

## Motivation

When running a small or single user mastodon instance you often use relays to get content to your instance and increase reachability of your posts.

This sadly increases the amount of content that reaches your instance from federated instances you may not want to host or spread.

See [the blog post](https://blog.thms.uk/2023/02/mastodon-block-tags) for more details.

## What the script is doing

- it searches for all posts containing defined hashtags and media attachements
- if the author of the post is not exempt (see below) they'll get suspended, and their data deleted.

### Exempting authors from suspension and deletion

This script can be run in single user, or multi user mode, which can be defined through the `multiUserMode` flag in `./config.sh`, and the option you choose will determine which users are exempt from suspension and deletion:

1. `multiUserMode=false`: In this mode your own account, as well as any accounts you personally follow are exempt from suspension and deletion. If you are running this against a multi user instance, this may cause accounts on your own instance to be deleted, as well as accounts followed by other users on your own instance, so it's probably only suitable for single user instances.
2. `multiUserMode=true`: In this mode any accounts on your instance, as well as any accounts followed by accounts on your instance are exempt from suspension and deletion. This is probably the better setup for most people.

## Setup

You can either run this locally on your machine, or as a GitHub Action:

### 1) Get the required access token:

Regardless of how you wish to run your script, you first need to register a new application in your mastodon instance:

1. Preferences > Development > New Application
   1. give it a nice name
   2. enable `read:account`, `admin:write:accounts`, and `admin:read:accounts`
   3. Save
   4. Copy the value of `Your access token`


### 2.a) Run as GitHub Action

If you wish to run this as a GitHub action:
1. Fork this repository
2. Adjust the settings in `./config.sh` for your need
3. Create an Actions Secret called `ACCESS_TOKEN`, and supply the token generated above:
   1.  Go to Settings > Secrets and Variables > Actions
   2.  Click New Repository Secret
   3.  Supply the Name and Secret
4. Finally go to the 'Actions' tab and enable the action.

### 2.b) Or run locally

Honestly, if you want to run this locally, you are probably better of using the original script, as it's a self-contained version, but if you do want run this version locally you could:

1. copy the script on your machine
2. Adjust the settings in `./config.sh` for your need
3. setup a cronjob that would run `bash /path/to/repo/block_hashtags.sh "{ACCESS_TOKEN}"`

## Epilogue
Both the idea and the original implementation are based on [@stefan@social.stefanberger.net](https://social.stefanberger.net/@stefan)'s script.

Find me on my Mastodon instance at [@michael@thms.uk](http://mstdn.thms.uk/@michael).
