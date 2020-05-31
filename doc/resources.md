# Resource Manager

The `ResourceManager` module provides a simple resource manager for videogames, currently geared towards 2D SDL games. It will parse a JSON file of the follow format:
```json
{ 
  "resources": [
    {
      "id": String,
      "path": String,
      "type": String
      "loadTriggers": [String]
      "unloadTriggers": [String]
    }
  ]
}
```

When created, the resource manager will load all of this data into memory, and will load any assets with the loadTrigger `"onStartup"`.

It currently supports the following types:
- `PNG`

You can get a resource with `get-r`, which will return a `ResourceData` instance, and there are special getters that will return default "error" assets if the requested asset is not available.

When you want to load an asset, you can do so by its unique id, or load assets in a batch by sending a "trigger". A trigger is essentially a tag on a resource. Triggers can be associated with characters, regions, levels, or whatever system of organization makes sense for your game. 

Right now, loading assets happens on the main thread and is blocking. In the future it will be non-blocking with a callback.
