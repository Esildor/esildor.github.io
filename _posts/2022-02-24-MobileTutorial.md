---
title: 'Mobile Game Tutorials in Unity'
date: 2022-02-24 00:00:00
featured_image: /images/blog/MobileTutorial/splash.png
excerpt: A quick rambling on the hard parts of building a common tutorial system within Unity
---

**tl;dr** 
This tutorial will show you how to create the basic UI for a mobile tutorial system within Unity3D. This tutorial will just show how to handle the UI bit of it. Mainly it explains how to do some of the painful UI setup and the calcuations for adjusting it accordingly. In Unity, everyone's UI is differnet, so this should be braodly applicable. As long as you have some sort of "Panel System" in your UI, where a single prefab represents a panel. It should be able to be adapted to whatever you might have.

Ever have one of the problems on during your softawre engineerig travels where its seems like you're the only one to ever solve the issue? I was recently asked to impliemnt and tutorial system within a project. Well I just did. So here's me giving back a bit 💖

For the uninitated, tutorial systems within mobile games follow a common trope. Its a bit hard to explain, so heres some examples:

<div class="gallery" data-columns="3">
	<img src="/images/projects/bau/showcase-1.jpg">
	<img src="/images/projects/bau/showcase-2.jpg">
</div>

Like shown above, it follows these core concepts:
* Overlay UI on top of the current panel to simulate the orginal panel being _"grayed out"_
* Provide some sort of _see through_ area. Notablly, this means the player can click through to some button or area
* (Optionally) Display some other UI elements over this grayed area

To my knowledge, in the UI world, this is called a **scrim**. If its not, [feel free to let me know](mailto:narkawiczsamuel@gmail.com). But for the reset of this post, I'll be refering to the grayed out area as such.

---

## Tutorial Panel Setup
One reason for making this whole post is to save ya'll some time with this part. The UI setup I'm about to show is critical to making the additional code below work. Here's how and why its setup this way.

### The 8 Boxes
The core of this is the follow 8 `RectTransforms` positioned and anchored in such a way that make it sutable for positioning around an arbitrary area or object.

<img src="/images/blog/MobileTutorial/overview.png">

Next note the hierarchy, and the boxes positions. Each point is anchored towards the center of the screen. Also, kinda goes without saying, but all of these `RectTransforms` have an image component on them. Color them dynaimcially or set the color of the the scrims to whatever you want.

<div class="gallery" data-columns="3">
	<img src="/images/blog/MobileTutorial/box-1.png">
	<img src="/images/blog/MobileTutorial/box-2.png">
	<img src="/images/blog/MobileTutorial/box-3.png">
	<img src="/images/blog/MobileTutorial/box-4.png">
	<img src="/images/blog/MobileTutorial/box-5.png">
	<img src="/images/blog/MobileTutorial/box-6.png">
	<img src="/images/blog/MobileTutorial/box-7.png">
	<img src="/images/blog/MobileTutorial/box-8.png">
</div>

Next up, create your script that'll manage this panel, I suggest placing it on the panel root, but since everything is referenced directly, I don't think it matters.
```csharp
  public class TutorialPanel : MonoBehaviour
  {
    public RectTransform Top, Bottom, Left, Right;
    public RectTransform TopLeftCorner, TopRightCorner, BottomLeftCorner, BottomRightCorner;
  }
```
Additionally, note that each rect has its width set to 1000. This is to ensure that it will strecht all the way off screen, regardless of the object its focusing on. Ideadlly, I would have had time to write a but more logic to make it fit within the bounds of the enclosing rect, but I never got to it. 😜
<img src="/images/blog/MobileTutorial/boxRect.png">
Now, the momment you've been waiting for! You don't have to set any of this up! Took me a good 30mins to set this all up, so here's a unity package with a prefab ready to go _(No script though)_! As loong as there's no major overhauls to the Unity UI system since the time of writing this, this should import fine with **any** version of Unity.
<a href="https://nbviewer.jupyter.org/github/Esildor/esildor.github.io/blob/master/files/Samuel-Narkawicz-Resume.pdf" class="button button--large">Download Unity Package</a>

## The not-so-magic Function
