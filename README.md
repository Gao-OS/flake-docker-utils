# flake-docker-utils

### Requirements: 
* NixOS
* Flakes
* Cross Compilation via [`boot.binfmt.emulatedSystems`](https://search.nixos.org/options?channel=unstable&show=boot.binfmt.emulatedSystems)


## flake-docker-utils

flake-docker-utils is a library that provides the following functions:

### `flake-docker-utils.lib.allImages`

#### Example:

``` nix
dockerImage = pkgs.dockerTools.buildImage { ... };

allImages = flake-docker-utils.lib.allImages [ "x86_64-linux" "aarch64-linux" ] dockerImage;
```

**Type** allImages :: [String] -> Derivation -> Derivation

#### Description: 

Builds a symbolic link farm for all systems to cross-compiled docker images.


### `flake-docker-utils.lib.pushAll`

#### Example:

``` nix
system = "x86_64-linux";

dockerImage = pkgs.dockerTools.buildImage { ... };

pushAll = flake-docker-utils.lib.${system}.pushAll {
  name = "pushAll";
  systems = [ "x86_64-linux" "aarch64-linux" ];
  inherit dockerImage;
};

```

**Type** pushAll :: AttrSet {String, [String], Derivation} ->
Derivation

#### Description:

Creates a script to push all images created with `allImages` using
`docker manifest`. `pushAll` uses the docker image's tag to tag every
image as `${tag}-${system}` and then includes them in a manifest with
`tag` as the manifest head.
