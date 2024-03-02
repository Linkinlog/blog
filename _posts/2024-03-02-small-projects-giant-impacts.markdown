---
layout: post
title: "Small Projects, Giant Impacts"
categories: learning
---

As craftsmen, we often strive for efficiency, but when it comes to learning, sometimes aiming big yields faster results. We have likely all heard the joke of someone using things like Next.js and K8s for a simple project such as a static site, yet I am here to argue that doing just that might be a 400 IQ move.

Our aim here isn't production-readiness or even practicality, it's the rapid absorption of concepts and workflows.

## What defines a small project?

Think of a project that you could build in a weekend or two. Your goal is a working prototype, not a polished product. When I was early on in my journey, I heard that not picking the most efficient tools in my projects would reflect poorly on my skills and ability to effectively pick tooling and concepts. Don't be afraid to cast aside conventional wisdom about these 'right' tools for the job. This project is your learning playground, it's okay to get messy!

## The humble todo app

A todo app is a surprisingly potent learning canvas and one that I reach for most often when I want to experiment. It can be as simple as a few lines of Javascript that use an array, or it can be a learning ground for a variety of technologies and concepts. You can practice [TDD](<https://en.wikipedia.org/wiki/Test-driven_development#:~:text=Test%20Driven%20Development%20(TDD)%20is,leading%20to%20more%20robust%20software.>) and [SOLID design](https://en.wikipedia.org/wiki/SOLID), breaking each step of the process into its layers when appropriate and starting with tests. It can be a distributed system with a Go backend for displaying the main page, a Rust backend for appending todos, a PHP backend for editing, and many more... We can learn how to interact with various DBMS, implement caching, and use tools like K6, Postman, and Puppeteer to simulate real users interacting with our site and see how well our project scales. We can make it a CLI app, a web app that communicates with GraphQL, gRPC, etc. A todo app is truly a versatile project.

## Build it all

Don't limit your toolbox! Another way I love to put this practice into motion is by turning common actions into tools. Start small - a template generator, a data formatter - but think about how your tool could evolve to assist others. Say you're doing coding challenges using a template and copying/pasting it for each challenge. Why not create a tool to generate it for you? You can open it up to multiple languages, styles, purposes, etc. And who knows, someone else might stumble upon it and find it useful.

Challenge yourself to pick a seemingly 'overkill' tech stack for your next mini-project. You might be surprised how quickly those skills solidify. The best way to learn is to Practice Enough New and Interesting Stuff and to get Time In The Saddle. These are some of my favorite ways to do both. And along the way, make sure to share your journey! Blog about your process, the struggles, and the breakthroughs. You'll inspire others to embrace this learning strategy.
