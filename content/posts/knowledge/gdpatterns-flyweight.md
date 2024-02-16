+++
title = 'GD Patterns: Flyweight'
date = 2024-01-22T13:05:50+04:00
draft = false
categories = ['Knowledge']
description = "Keeping it light."
tags = ['unity', 'csharp', 'architecture']
+++

A continuation of the _GD Patterns_ series, following the _Game Programming Patterns_ [book](https://gameprogrammingpatterns.com) by Robert Nystrom, and applying its insights within the Unity environment.

## Flyweight

[Flyweight](https://gameprogrammingpatterns.com/flyweight.html) pattern is all about efficiency. It's essential when a game relies on large quantities of similar objects in a scene. The core idea is straightforward—reduce the memory footprint by sharing common data.

### Example usage

Consider we have an original idea for a survival game, where the entire world consists of voxel blocks with different properties. The primary focus of the game is mining these blocks and crafting stuff from them.

In order to populate such a world, we might start by defining a `Block` class.

```csharp
public class Block : MonoBehaviour
{
    [SerializeField] private Mesh mesh;
    [SerializeField] private Texture tex;
    [SerializeField] private Texture breakTex;
    [SerializeField] private AudioClip breakAudio;
    [SerializeField] private bool isBreakable;
    [SerializeField] private bool health;
    [SerializeField] private Vector3 position;
    // etc.
}
```

Here, I'm marking each field with a `SerializeField` attribute to enable assigning values from the editor.

Our `Block` class contains a substantial amount of data (and the list is not exhaustive). Having thousands of instances of the class may cause a heavy load on both memory and the GPU, as each block would feed it with its vertices and texture data every frame. Creating Block prefabs won't help much, as the prefab keeps its own copy of data when instantiated.

Upon closer inspection, we notice that not all fields need to be exclusive to the object instance; some of them can be shared. Let's assume that each block in our game has the same mesh, breaking texture, and breaking audio. Then, we extract these fields into a separate `BlockModel` class and maintain its reference in the `Block`.

```csharp
public class BlockModel
{
    public Mesh Mesh;
    public Texture BreakTex;
    public AudioClip BreakAudio;
}

public class Block : MonoBehaviour
{
    // instance specific data
    [SerializeField] private Texture tex;
    [SerializeField] private bool isBreakable;
    [SerializeField] private bool health;
    [SerializeField] private Vector3 position;

    // shared data
    private BlockModel _blockModel;
}
```

- The unchanging fields common to all instances of a class are referred to as the intrinsic state. It is crucial to ensure that these fields are immutable.
- The fields unique to each instance of a class, are called the extrinsic state.

For the sake of convenience, we can make the `BlockModel` a `ScriptableObject`, which makes creation and editing of different models fast and easy, ensuring its data is shared between different instances without copying.

```csharp
[CreateAssetMenu(fileName = "BlockModel", menuName = "ScriptableObjects/NewBlockModel", order = 1)]
public class BlockModelSO : ScriptableObject
{
    public Mesh Mesh;
    public Texture BreakTex;
    public AudioClip BreakAudio;
}
```

{{< figure src="img/knowledge/flyweight_so.png" alt="Flyweight SO" position="center" style="border-radius: 8px;"  >}}

In case flyweights are created dynamically and requested on demand, it would be a good idea to hide the construction behind an abstraction and delegate the creation of flyweights to a factory. The Pool pattern can assist in keeping track of already constructed flyweights. 

### Rendering optimization

Now, thousands of blocks in our game share common data, significantly improving memory usage. However, the GPU load remains unchanged—instanced rendering is here to help, and it is itself an implementation of the Flyweight pattern.

[Instancing](https://en.wikipedia.org/wiki/Geometry_instancing) is a technique that allows rendering multiple copies of a mesh within a single draw call, supported natively by graphic card vendors.

In the Unity environment, this feature is known as GPU Instancing. To use it, you have to apply shaders with _Enable GPU Instancing_ option, or edit your own shaders to support the feature. Detailed instructions and restrictions can be found in the Unity [documentation]((https://docs.unity3d.com/540/Documentation/Manual/GPUInstancing.html)).

{{< figure src="img/knowledge/gpuinstancemat.png" alt="Enable GPU Instancing" position="center" style="border-radius: 8px;"  >}}

Another technique to optimize rendering is draw call batching. Using this method, meshes with shared materials can be combined, resulting in a lower draw call count. There is static batching for static geometry, dynamic batching for small dynamic objects, and the Scriptable Render Pipeline (URP, HDRP) includes a SRP Batcher, which is used by default.

There's a lot to unpack here, with each technique having its own implications. Performance-wise, Catlike Coding has an extensive [tutorial](https://catlikecoding.com/unity/tutorials/basics/measuring-performance/) on profiling in Unity, covering GPU Instancing and Batching as well.
