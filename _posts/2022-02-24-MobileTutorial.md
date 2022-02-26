---
title: 'Mobile Game Tutorial Highlighting in Unity'
date: 2022-02-24 00:00:00
featured_image: /images/blog/MobileTutorial/splash.gif
excerpt: A quick rambling on the hard parts of building a common mobile game system tutorial system within Unity
---
**tl;dr**
This tutorial will show you how to create the basic UI for a mobile tutorial system within Unity3D. Mainly it explains how to do some of the painful UI setup and the calculations. In Unity, everyone's UI is different, so this should be broadly applicable. As long as you have some sort of "Panel System" in your UI, where a single prefab represents a panel. It should be able to be adapted to whatever you might have.

Ever have one of the problems on during your software engineering travels where its seems like you're **the only one** looking for discussion on the topic? I was recently asked to implement and tutorial system within a project. And it seemed like no one online was trying to solve the same thing. Well I had some hiccups and since I couldn't find any documentation on it. So I decided to write this up and give back to the community. 💖

For the uninitiated, tutorial systems within mobile games follow a common trope. Its a bit hard to explain, so heres some examples:

<div class="gallery" data-columns="3">
	<img src="/images/blog/MobileTutorial/example-1.png">
	<img src="/images/blog/MobileTutorial/example-2.png">
	<img src="/images/blog/MobileTutorial/example-3.png">
	<img src="/images/blog/MobileTutorial/example-4.png">
</div>

Like shown above, it follows these core concepts:
* Overlay UI on top of the current panel to simulate the below panel being _"grayed out"_.
* Provide some sort of _see through_ area. Notably, this means the player can click through to some button or area.
* (Optionally) Display some other UI elements over this grayed area
To my knowledge, in the UI world, this is called a **scrim**. If its not, [feel free to let me know](mailto:narkawiczsamuel@gmail.com).

---

## Goal:
Make a tutorial panel like the ones shown above. Allow developers to _"focus"_ on an arbitrary rect transform from another UI element. A strong emphasis being that developers should **not** have to modify the gameplay panels just to add a tutorial focusing to it.

---

## Tutorial Panel Setup
One reason for making this whole post is to save ya'll some time with this part. The UI setup I'm about to show is critical to making the additional code below work. Here's how and why its setup this way.

### The 8 Boxes
The core of this is the following 8 `RectTransforms` positioned and anchored in such a way that make it suitable for positioning around an arbitrary target object.

<img src="/images/blog/MobileTutorial/overview.png" width="300">

Next note the hierarchy, and the boxes positions. Each point is anchored towards the center of the screen. Also, kinda goes without saying, but all of these `RectTransforms` have an image component on them. Color them dynamically or set the color of the scrims to whatever you want. Also the image components should have there `Raycast Target` field checked so other input is prevented.

<div class="gallery" data-columns="4">
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
Additionally, note that each rect has its width set to 1000. This is to ensure that it will stretch all the way off screen, regardless of the object its focusing on. Ideally, I would have had time to write a bit more logic to make it fit within the bounds of the enclosing rect, but I never got to it...
<img src="/images/blog/MobileTutorial/boxRect.png">
Now, the moment you've been waiting for! You don't have to set any of this up! Took me a good hour to figure out and set this all up, so here's a unity package with a prefab ready to go! _(No script though)_. As long as there's no major overhauls to the Unity UI system since the time of writing this, this should import fine with **any** version of Unity.

<a href="https://www.github.com/Esildor/esildor.github.io/blob/master/files/tutorialPanel.unitypackage" class="button button--large">Download Unity Package</a>

## The Focus Function
Finally, we'll add some more logic to the `TutorialPanel` script the we'll actually call when we want to focus on some object. This function is basically my whole reason for making this post. Since I'm a bit dumb, and mostly guessed and checked my way here. I figured I should do anything I can to save someone else the time. So here ya go.
```csharp
public void FocusOnTarget(Canvas canvas, float padding, RectTransform focusTarget)
{
  Vector3[] corners = new Vector3[4];
  focusTarget.GetWorldCorners(corners);

  Vector3 top, bottom, left, right;
  Vector3 topLeftCorner, topRightCorner, bottomLeftCorner, bottomRightCorner;
  // Set the corner points and apply position
  topLeftCorner = new Vector3(corners[1].x - padding, corners[1].y + padding, 0);
  topRightCorner = new Vector3(corners[2].x + padding, corners[2].y + padding, 0);
  bottomLeftCorner = new Vector3(corners[3].x + padding, corners[3].y - padding, 0);
  bottomRightCorner = new Vector3(corners[0].x - padding, corners[0].y - padding, 0);
  TopLeftCorner.transform.position = topLeftCorner;
  TopRightCorner.transform.position = topRightCorner;
  BottomLeftCorner.transform.position = bottomLeftCorner;
  BottomRightCorner.transform.position = bottomRightCorner;
  // Set the middle points and apply position and padding
  top = ((topRightCorner + topLeftCorner) / 2);
  bottom = ((bottomLeftCorner + bottomRightCorner) / 2);
  left = ((topLeftCorner + bottomRightCorner) / 2);
  right = ((topRightCorner + bottomLeftCorner) / 2);
  // Calculate the width (remember, Left and Right are rotated, thats why 
  // we're not setting the height) of the boxes. Based on distance accounting
  // for scale.
  float topAndBottomWidth = Vector3.Distance(topLeftCorner, topRightCorner) / canvas.scaleFactor;
  float leftAndRightWidth = Vector3.Distance(topLeftCorner, bottomRightCorner) / canvas.scaleFactor;
  Top.transform.position = top;
  Top.sizeDelta = new Vector2(topAndBottomWidth, Top.sizeDelta.y);
  Bottom.transform.position = bottom;
  Bottom.sizeDelta = new Vector2(topAndBottomWidth, Bottom.sizeDelta.y);
  Left.transform.position = left;
  Left.sizeDelta = new Vector2(leftAndRightWidth, Left.sizeDelta.y);
  Right.transform.position = right;
  Right.sizeDelta = new Vector2(leftAndRightWidth, Right.sizeDelta.y);
}
```

### Walking through the function
Trying to keep things logical, going to walk through some bits of it, top to bottom:
#### Three parameters:
- `Canvas canvas` is the just to handle scaling. If you're drawing the tutorial panel and object you're focusing on are using two different panels, well idk, good luck. If the scales are the same, it should be fine
- `float padding` is pretty straight foreword. As stated in the **goal**, we don't want to have to modify the game panel or the object we are focusing on, but we also don't want to highlight the buttons and none of the surrounding area. So this is just mainly for ease of use.
- `RectTransform focusTarget` is the object you want to highlight.

#### That Unity sauce
`focusTarget.GetWorldCorners(corners);` is the true magic here, you can find the docs [for it here](https://docs.unity3d.com/ScriptReference/RectTransform.GetWorldCorners.html). But the long and short of it is, that this method does some math for you to determine the positions of the corners of your rect, in world space.

#### Pulling my hair out for you
Note how I'm setting the `transform.position` on the objects, as opposed to doing the more standard practice of modifying the `RectTransform`. This is because we are getting the **world** position from the `GetCorners()` method. One of the great things about this method of doing this is that it doesn't care about anchoring or positioning of the target rect. Since the he `GetCorners()` method ignores all of that, so regardless of the complexity of your UI (like anchoring, nested in other anchors, etc) it should be able to focus on it. 

---

### The result:
I used a bit of Odin Inspector goodness to make this button to test it at editor-time. You could just try it at runtime, or create your own editor script. 
<img src="/images/blog/MobileTutorial/final.gif" width="300">

---
This is my first time ever doing something like this, so if you found this help, let me know I'm not insane by giving me a _thanks_ email. Also if you have any questions on the matter, feel free to [shoot me an email for that too](mailto:narkawiczsamuel@gmail.com)!