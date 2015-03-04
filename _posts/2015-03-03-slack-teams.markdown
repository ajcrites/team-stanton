---
layout: post
title:  "Slack teams"
date:   2015-02-14 13:41:00
category: process
author: gotoplanb
---

tl;dr You need to self-signup for a second Slack team (https://moball.slack.com) and ensure you've updated your Slack app to allow multiple teams. Location and HR channels migrate to MobAll. We keep MobDev for platform channels and project groups.

## The challenges

In the effort to standardize our chat channels, Slack has emerged as the overwhelming favorite. However, edge cases and limitations are starting to show.

### Message history is only a few days

Although Slack should not be considered archival, it is imperative that chat history last at least 14 days to accommodate 2-week sprints.

### Notification queuing is nebulous

We have so many channels for different reasons that is has become very difficult to be in many channels and also be able to properly queue your attention toward notifications.

### Project communication is fragmented

Dev and QA are in Slack but we need to also include the full project crew -- design, architecture, PM, etc. This introduces more challenges in terms of sandboxing information. Many projects have third-party vendors, contractors and client representatives that need to be in the team chat as well.

### Limited integrations

Slack allows 10 types of integrations per team. While we have some common integration types needed for all project (e.g. GitHub), we quickly max out on optional integrations like Google Docs, Dropbox, Asana, custom webhooks, etc.

Some of these challenges could be solved by paying for Slack for everyone in Mobiquity but not all. 

## The Plan

Here's the plan that has been in limited testing mode for the last 2 weeks:

### Everyone joins a MobAll team

We move the location, hr, finance and direct-message communication to this team. Everyone in Mobiquity will be in this team regardless of job function. This is the place for 1:1 banter, lunch, trivia, etc.

### MobDev remains the place for platform channels 

Platforms (e.g. #plat-js), job roles (e.g. #role-qa) and other channels directly related to the dev and qa organization say in MobDev. Resource-manager channels will become private groups. For now, all project groups will remain here but will be deprecated

### New projects will trigger creation of a new project-specific Slack team (e.g. mob-p-jwa.slack.com). 

Project-specific Slack teams mean you can have 10 integrations customized per project (yay!) and better accommodate full team inclusion. If a project warrants paid usage, we now have better segmentation, so that we can bill to the project directly.

The project-specifc teams will have standardized names for private groups to handle the full spectrum of project roles. There will be a #general used for communicating with non-mobiquity members (client, vendors, etc.) as appropriate and managed by the tech lead.

### Example table for new channel partitioning

| MobAll      | MobDev       | MobP JWA     |
|-------------|-------------|---------------|
|#general     | #android     | #general     |
|#finance     | #ios         | architecture |
|#hr          | #javascript  | design       |
|#loc-gnv     | #random      | dev          |
|#loc-gnv-618 | #onramp-ducks| qa           |
|             | team-stanton | support      |

## Timeline and transition actions

MobAll and MobDev will just be a partitioning of the channels you know and love. Turn off all notifications on MobAll and use as your digital water cooler instead of a distraction machine.

The Projects part of the overall plan is going to be rolled out slowly and with some further training as part of your kickoff meetings. 