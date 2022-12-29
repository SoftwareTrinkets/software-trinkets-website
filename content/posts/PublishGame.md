---
title: "Publishing a game on Itch.io"
date: 2022-12-24T11:09:49+01:00
draft: true
# toc: false
tags:
  - BabylonJS
  - Game
  - Itch.io
---

## Introduction

In the [last post](./BabylonJS) I've made a small game, but what use is a game if noone can play it? So in this post I'm going to document how to publishing something like this. I've never done that before with BabylonJS, so we're going to find out how that'll work. I have the following rough plan:
- generate a new babylon project on my local machine
- have it 'build' a html page??
- upload that to Itch.io
But we'll see what actually happens

## Immidiately adding to the scope of the game
So something that bugged me while showing my game to other is that I can run it on my phone, but I can't interact. It's such a small thing to fix, let's quickly include that. I'm going to do this super quick and not very clean since this wasn't the plan. 
Here's what I added to make it work: 

    scene.onPointerObservable.add((pointerInfo) => {
      if (pointerInfo.type === BABYLON.PointerEventTypes.POINTERTAP) {
          if (pointerInfo.event.offsetX / canvas.width < (1/3)) {
              if(score < 50){
              BABYLON.Animation.CreateAndStartAnimation("boxmove", box, "position.x", 60, 5, box.position.x, -2, 0);
              } else{
                  box.position.x = -2
              }
          } else if (pointerInfo.event.offsetX / canvas.width > (1/3) && pointerInfo.event.offsetX / canvas.width < (2/3)){
              if(score < 50){
              BABYLON.Animation.CreateAndStartAnimation("boxmove", box, "position.x", 60, 5, box.position.x, 0, 0);
              } else{
              box.position.x = 0
          }
          }else if (pointerInfo.event.offsetX / canvas.width > (2/3)){
              if(score < 50){
              BABYLON.Animation.CreateAndStartAnimation("boxmove", box, "position.x", 60, 5, box.position.x, 2, 0);
              } else{
                  box.position.x = 2
              }
          }
      }
  });

[Playground](https://playground.babylonjs.com/#DSH9NF#9)

## Back to business
Okay, no more scope changes, for real this time. Let's create a local 