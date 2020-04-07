---
layout: post
title: "[TIL][Golangtw] note for Dave Cheney session in Golangtw"
description: ""
category: 
- TodayILearn
- Golang
tags: ["TIL", "golang"]

---

## Meetup related material



- [slide](https://go-talks.appspot.com/github.com/davecheney/understanding-the-execution-tracer/understanding-the-execution-tracer.slide#1)
- [sample code](https://github.com/davecheney/understanding-the-execution-tracer)

## Meetup note

pprof -> no clue about how many CPU we used.
trace -> understand the process time and 
channel send -> channel recev

"Synchronization blocking profile" is a good clue to figure out problem related to unbuffered channel.

Execution-tracer also help to figure out problem when you trace running http server.
It could help us to figure out network request and http serve diagram.

View Option -> Flow Arrow  to display all the  incoming and outcoming flow.

Goroutine announce -> to understand the goroutine path

Execution tracker is low overhead, until the lots golang routine waiting (not must but almost 10%).


