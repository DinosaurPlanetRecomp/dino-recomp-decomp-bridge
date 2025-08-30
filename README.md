# Dinosaur Planet: Recompiled - Decomp Bridge
This repository bridges the gap between [Dinosaur Planet: Recompiled](https://github.com/DinosaurPlanetRecomp/dino-recomp) and the [Dinosaur Planet Decompilation](https://github.com/zestydevy/dinosaur-planet) project by providing:

- Symbol definitions
- Decompilation headers
- Recomp aliases for Dinosaur Planet DLL symbols

This is necessary for running static recompilation, creating patches, and for creating recomp mods.

## Overview
- `dinosaur-planet` - The Dinosaur Planet Decompilation project. The headers from the decomp can be used to create patches and mods. Symbols provided by the Decomp Bridge are generated from this submodule.
- `dino.syms.toml` - Function symbols for core and DLL code.
- `dino.datasyms.toml` - Global/static variable (data) symbols for core and DLL code.
- `dino.datasyms_manual.toml` - Not all data symbols can be automatically extracted from the decomp. This file contains manually mapped symbols for those that weren't automatically mapped.
- `dino.dlls.txt` - A list of all DLL section names as used in the previously mentioned TOML files.
- `include/recomp` - Headers containing aliases (`#define`s) for symbols defined within each DLL. DLLs in Dinosaur Planet are their own linkage units and as such, symbols defined in DLL code are completely isolated from other DLLs and the core code. Because of this, the decomp is allowed to contain ambiguous symbol names across the DLLs and core code. Recomp on the other hand does not support this and requires that all symbols across the entire game be unique. When symbols from DLLs are mapped for recomp, they are prefixed with `__dll{DLL Number}_` to achieve this, however the decomp headers do not use this prefix which makes using the headers difficult for DLL patches. Including one of these alias headers will use C `#define`s to convert the symbol names to their recomp versions in code.

## Updating Headers and Symbols
The provided symbols and headers are tied to a specific commit of the decompilation project. If you wish to use newer headers from the decomp, you **must** also regenerate the recomp symbols and recomp DLL alias headers.

### 1. Update the Decomp Submodule
Using Git, checkout the commit of the decomp you wish to update to in the `./dinosaur-planet` submodule. Be sure to also run `git submodule update --recursive` from **within** the submodule directory to ensure Git submodules are up-to-date.

### 2. Build the Decomp
Follow the build instructions in [the decompilation readme](./dinosaur-planet/README.md). The build should be done from the submodule and not an external clone of the repository. If you have done a build previously, consider doing a full clean + extract + build cycle. An up-to-date `build/dino.elf` file is required for the next step.

### 3. Regenerate Symbols and Headers
Run the following tool (from this directory) to regenerate symbols and recomp DLL headers:
```bash
python ./dinosaur-planet/tools/gen_recomp_syms.py
```

> [!NOTE]
> If you used a Python virtual environment to build the decomp, make sure to run the above command from within that virtual env.
