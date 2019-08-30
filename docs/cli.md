---
id: cli
title: CLI
sidebar_label: CLI
---

Newton comes with a command line interface to easily scaffold projects.
The syntax is generally `newton [COMMAND] [args]`

## `startproject`

In order to start a new Newton project, run `newton startproject [YOURPROJECTNAME]`.
This will create your `app.js` file which is your main server file, as well as a `main` directory, which will contain configuration files. It will also create a `package.json` file with the dependencies you need.

## `startapp`

A Newton project, like Django, is made up of smaller apps. To create a new app, run `newton startapp [YOURAPPNAME]` within your Newton project directory.
