---
title: 'Understanding Unity Asset Serialization Behavior: Why the hell are all these files here?'
date: 2024-04-08 00:00:00
featured_image: /images/blog/UnityAssetSerializationBehavior/splash.png
excerpt: "*Stares at Unity and all the changed assets in git* "Why the hell did all these asset change?" "Why do my assets change like they've got a mind of their own?""
---
If you've been working with Unity for some time, you might have noticed assets changing seemingly at random. I've had many co-workers over the years ask:
> Hey, how come when I change this _(insert asset, prefab, scene, etc)_ all these changes and new values appear in my source control diff?

I've answered this question so many damn times, I decided to write a blog post about it. First off, its good that they are asking this. Their being smart devs and not just checking in _random_ changes to the project, and instead inquiring about it first. I was curious about this too one day and looked into it. In doing so, I learned a bit more about Unity's serialization system works. There's a method to this madness, and it all comes down to how Unity handles asset serialization.

## What prompts this change
Unity only updates the serialization of an asset when it needs to. For example, let's say you've just added a new serialized field to a script: 
```csharp
public string something;
```
Lets assume this script is inherits a `ScriptableObject`, or a `Monobehaviour`. In the case of the `Monobehaviour`, you should also assume the **extremely common case** that this script is attached to a prefab or an object in the scene. Unity won't serialize that value into the file immediately. Ask yourself, why should it? Currently, you haven't set it to **anything** in the inspector. So functionally it's just a `null` string. Since its a default value, when Unity finds this new `something` field when loading the asset off disk at runtime, its just going to slap that same default value on there. It just saved both time, memory, and disk space! 😎

## When does Unity update these serialized values?

Ok so you've added this serialized field. Lets say this script is attached to tons of objects/prefabs. For Unity to update all of these, it would have to troll over all the assets, scenes, and prefabs in the project. Then check if that script was serialized, then reserialize it with the default value. That would take a ton of time, and produce a ton of changes.
Again, **not** having that value serialized doesn't gain you anything! 

Until... You change _something_ on that object!

So weeks later you come back to that random prefab. Assign a new value to a random field, then you look at that asset in git. You'll likely see your old `something` field newly serialized! It makes a bit more sense for Unity to do it now, since it was re-serializing the whole asset again anyway.

What can make this _more confusing_, is that this same rule applies for **other** developers changes. So could be seeing values getting re-serialized that are from someone else's changes. Leaving you scratching your head.

Lastly, take this concept and zoom it out further. This same rule applies to meta files, and any scripts within the project, including assets containing serialized objects from **packages** or **Unity's Engine** objects. This is why, when an engine or package upgrade happens, you might start seeing a ton of assets _changed_ 👀

## How I handle new asset serialization

First off, if you don't know what you're doing, its probably safe to just check in those changed values on an asset. Done.

Now, to minimize this and keep a clean project
```csharp
AssetDatabase.ForceReserializeAssets();
AssetDatabase.SaveAssets();
```
Warning: This operation can be time-consuming, sometimes taking an hour or more, depending on the size of your project. This is because it forces **everything** to be re-serialized. However, doing it during engine upgrades, ensures that all changes are captured, including those in materials, meta files, and assets.

In conclusion, Unity's asset serialization behavior may seem random at first, but it's a deliberate process designed to ensure that your projects remain compatible and consistent across upgrades. By understanding this behavior and following best practices, you can manage asset changes effectively and avoid unexpected issues in your Unity projects.

---
Second _technical-ish_ post! If something in here is incorrect, or you found this helpful, let me know I'm not insane by shooting me an [email](mailto:narkawiczsamuel@gmail.com)!