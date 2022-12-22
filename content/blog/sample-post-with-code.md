---
date: 2022-11-21
title: Sample Article with Code
categories: ["articles"]
keywords:
    - the web
    - performance
    - sample
    - code
summary: |
 Sample post with code guide
---


`{{ if (isset .Params "description") }}
    {{ index .Params "description" }}
{{ else if (isset .Params "summary") }}
    {{ index .Params "summary" }}
{{ else }}
    {{ .Summary }}
{{ end }}`

### Unity tooling for VR
This time we utilized an official package - Unity XR Interaction Toolkit. So far this is the best solution I have used in Unity for interfacing and working with VR headsets. The components are well-designed, documented and extensible. The best part is that they work really well with the new Input System Unity provides.

However, during development we faced some challenges when deploying for different VR headsets, where we had to utilize the OpenXR Plugin. For the majority of the time, we had no issues with Oculus Rift S or Oculus Quest headsets. Though building for SteamVR (we also targeted HTC Vive) posed a lot of problems. The virtual hands would be offset differently for each headset, input bindings would not work or weird overlay textures would appear.

We resolved some of these problems by installing Beta versions of the Oculus and SteamVR runtimes. Yet to fully fix them, we had to create two different builds for each runtime. As it stands now, the plugin is not yet ready for production when targeting multiple vendors. If you encounter some of these issues, [Unity support for OpenXR in preview][other-forum-xr] thread might provide helpful solutions.

### Loading multiple scenes at once
When working with VR (and in general any game) in Unity an essential feature is smoothly transitioning between scenes. Simply loading a scene via `SceneManager.LoadScene` after fading out the screen introduces a noticeable stutter which in some cases can prompt the VR runtime to display a loading overlay which ruins the gameplay experience. The usual approach for this is to utilise `SceneManager.LoadSceneAsync` with `LoadSceneMode.Additive` in addition to always keeping one scene open which acts as an intermediate scene for storing the VR components.

For this project we followed a similar approach to the one used in [Gneiss][other-gneiss], where we always keep one scene open for storing various gameplay systems. This time however, we utilised Multi-Scene editing feature which made the experience of editing two scenes at the same time a lot smoother. The only challenge was to make sure that each time after opening a `.scene` asset the appropriate scenes would load. To solve this we utilised `InitializeOnLoad` and `OnOpenAsset` attributes to hook into Unity's callbacks. You can see this in action in [SceneBootstrapLoader.cs][project-scene-bootstrap].

Read all 39 tips on [@krabf](https://www.krabf.com).