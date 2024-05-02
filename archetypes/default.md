---
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
description: 'Post di paco'
date: {{ .Date }}
draft: true
tags: ['']
image: 'posts/{{ .File.ContentBaseName }}/images/IMAGE_NAME'
cover:
  image: 
  caption:
---

<!-- USEFUL STUFF -->
<!-- NOTE: Put the audio files in the same dir of index.md -->
<!-- {{<audio img-src="images/<COVER_IMAGE>" src="posts/<POST_NAME>/<AUDIO_NAME>" width="100%" caption="<AUDIO_NAME>" >}} -->

<!-- {{< youtube "<YOUTUBE_VID_ID>" >}} -->