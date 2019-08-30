---
id: GettingStarted
title: Getting Started
sidebar_label: Getting Started
---

Newton is a web framework that builds on of [Express.js](https://expressjs.com), inspired by [Django](https://www.djangoproject.com).
It can be difficult to quickly get an Express.js project up and running, so Newton was built to make it easier to quickly create a project.
By default, it uses [MongoDB](https://www.mongodb.com/) (with [Mongoose](https://mongoosejs.com/)) and runs at port 8000, but it can be easily configured.

## Installation
First, install the newton-cli via:

`npm i @newton-web/newton`

Next, go to a directory that you want to create a project and create a project:

`newton-admin startproject NewtonProject`

Next, cd into the directory and `npm install`, then run the server:

```bash
cd NewtonProject
npm install
nodemon app.js
```

If you go over to `localhost:8000`, you should be greeted with Newton's welcome message.
You can change your port in your `NewtonProject/main/settings.js`;
