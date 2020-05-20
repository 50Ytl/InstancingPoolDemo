# InstancingPoolDemo
Instancing Pool Demo is a lightweight data oriented laser projectile solution for Unity using DrawMeshInstanced

This project contains the data oriented laser projectile solution I designed for my [Disputed Space](https://store.steampowered.com/app/637410/Disputed_Space/) and [Allspace](https://store.steampowered.com/app/1000860/Allspace/) games, which are available through Steam.  This solution runs in Unity 5.5 and newer, and relies on the [DrawMeshInstanced](https://docs.unity3d.com/ScriptReference/Graphics.DrawMeshInstanced.html) method in the Unity API for rendering using GPU Instancing.  This solution delivers about six times the performance of a well written object pool, and this solution does not create additional game objects for each laser projectile.  I have tested this code in Unity 5.5, Unity 2018.4, and Unity 2019.3.

I am making this available because other game developers have asked to see the code.  If you are a game developer and you are making a new project right now, I actually recommend using Unity's official DOTS solution instead of this code, but you can use this in your game projects if you want to.  Just remember that this is an excellent example of a light weight data oriented solution, but this is not intended to be a complete DOTS/ECS style solution.

### Create Mesh and Material using Meshbaker
In a normal object pool, you would create a prefab game object for the object pool to spawn.  That is not how an instancing pool works, though.  With an instancing pool, you will render using the DrawMeshInstanced method instead of using game objects.  When you use the DrawMeshInstanced method in Unity, you need to supply a single mesh and single material.  

I initially designed a simple projectile game object using three textured quads.  Then I used [MeshBaker](https://assetstore.unity.com/packages/tools/modeling/mesh-baker-5017) to bake the multiple textured quads into a single new game object in an empty scene.  That created game object which is not used, and it creates a mesh and material that are used.  Then I used the mesh and material generated by MeshBaker to supply the DrawMeshInstanced method.  I have included mesh, material, texture, and shader examples in this GitHub repository.  This instancing pool demo include the small blue lasers, red lasers, and green plasma that are seen in my space games on Steam.

In my [Disputed Space](https://store.steampowered.com/app/637410/Disputed_Space/) and [Allspace](https://store.steampowered.com/app/1000860/Allspace/) games, I also used MeshBaker to prebake additional weapons including turrets with multiple lasers.  For example, some laser turrets in Disputed Space have three or four gun barrels.  I prebake the lasers from all of those gun barrels into a single mesh and material combo.  That way a triple or quad laser turrent only launches a single projectile each time all of the gun barrels fire together.  This is an additional way I increased the number of lasers in the scene while decreasing draw calls.

### InstancePoolLasersBaseClass.cs
The main code for my data oriented laser projectile system can be found in the [InstancePoolLasersBaseClass.cs](https://github.com/ShilohGames/InstancingPoolDemo/blob/master/Assets/ShilohGames/InstancingPool/Scripts/InstancePoolLasersBaseClass.cs) class.  Classes like LasersBluePool.cs and LasersRedPool.cs actually inherit the InstancePoolLasersBaseClass.cs class.  If you want to do impact effects and apply damage to targes, add that code to the InstancePoolLasersBaseClass.cs class.

### Referencing the Resources
In your scene, create a game object to hold the LasersBluePool.cs and LasersRedPool.cs scripts.  Then assign the correct material and mesh to each script.  Make sure the materials are using the correct textures and shader.  The included shader is simply an instancing enabled version of the additive particle shader that shipped with Unity 5.5.

### Firing Lasers
In a script on your space ship game object in your own space game, use the following code to fire a blue laser.
```
LasersBluePool.current.FireWeapon(transform.position, transform.rotation, transform.forward, gameObject);
```

### Bloom
I did not include any Bloom post processing solution in this example.  I strongly recommend using Bloom, as it makes the laser projectiles look a lot more impressive.  I used the Bloom feature in the [Beautify](https://assetstore.unity.com/packages/vfx/shaders/fullscreen-camera-effects/beautify-61730) assset with my space games.

### Performance
When I tested this instancing pool demo project in Unity 2019.3 on my computer, I was getting over 900 FPS with 3-5 draw calls.  My games on Steam typically get about 100-300 FPS depending on the user's computer hardware specs.
