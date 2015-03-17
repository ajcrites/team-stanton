---
layout: post
title:  "Slack teams"
date:   2015-02-14 13:41:00
category: process
author: gotoplanb
---

tl;dr You need to self-signup for a second Slack team [https://moball.slack.com](https://moball.slack.com) and ensure you've updated your Slack app to allow multiple teams. Location and HR channels migrate to MobAll. We keep MobDev for platform channels and existing project groups. You will be invited to new project groups by your tech lead.

## The challenges

In the effort to standardize our chat channels, Slack has emerged as the overwhelming favorite. However, edge cases and limitations are starting to show.

### Message history is only a few days

Although Slack should not be considered archival, it is imperative that chat history last at least 14 days to accommodate 2-week sprints.

### Notification queuing is nebulous

We have so many channels for different reasons that it has become very difficult to be in many channels and also be able to properly queue your attention toward notifications.

### Project communication is fragmented

Dev and QA are in Slack but we need to also include the full project crew -- design, architecture, PM, etc. This introduces more challenges in terms of sandboxing information. Many projects have third-party vendors, contractors and client representatives that need to be in the team chat as well.

### Limited integrations

Slack allows 10 types of integrations per team. While we have some common integration types needed for all project (e.g. GitHub), we quickly max out on optional integrations like Google Docs, Dropbox, Asana, custom webhooks, etc.

Some of these challenges could be solved by paying for Slack for everyone in Mobiquity but not all. 

## The Plan

Here's the plan that has been in limited testing mode for the last 2 weeks:

### Everyone joins a MobAll team

We move the location, hr, finance and direct-message communication to this team. Everyone in Mobiquity will be in this team regardless of job function. This is the place for 1:1 banter, lunch, trivia, etc.

This partitioning will let us keep a longer message history because our volume of messages is split across multiple teams. We also empower project teams to choose integrations and external members that help set the project on a path to success.

### MobDev remains the place for platform channels 

Platforms (e.g. #plat-js), job roles (e.g. #role-qa) and other channels directly related to the dev and qa organization stay in MobDev. Resource-manager channels will become private groups. For now, all project groups will remain here but will be deprecated

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

## Have use-case questions?

Please make a comment on this [GitHub issue](https://github.com/Mobiquity/team-stanton/issues/12) with any use-case questions you have. There will be bumps in the road no doubt.

### How do I make a call?

Skype and Hangouts provide the opportunity to make calls to groups. While Slack does not have a native dialing function. Fear not! Slack integrations let's us just type `/hangout`, click the link, and instantly you are transported into a world of amazement and wonder! Well, at least you can make an on-the-fly Google Hangouts link that everyone in your channel/group can see. Sweet! Have other ideas for calling features while we wait for Slack to complete the announced built-in functionality? Post a comment on this [GitHub issue](https://github.com/Mobiquity/team-stanton/issues/12).

### Where should I send a direct message?

* *Is it related to a specific project?* First consider if the message is really private. Likely you want to use an `at-mention` to a person within the project channel. This provides everyone on the team with visibility (if they want it) and the ability to search back in the history if needed later.
* *Really, I promise this should be private but it is related to a project?* Send a direct message in the project Slack team. Folks on legacy projects should just send direct messages in MobAll.
* *This is not related to a project.* Post in MobAll team.
* *When should I post a direct message in MobDev?* Probably never. I haven't thought of a proper use case yet, but do let me know as you find opportunities to improve how we use Slack.

## How do I get started?

You need to self-signup for a second Slack team [https://moball.slack.com](https://moball.slack.com) and ensure you've updated your Slack app to allow multiple teams. You will be invited to new project groups by your tech lead.
