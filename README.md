# ComfyUI-0246
Random nodes for ComfyUI I made to solve my struggle with ComfyUI. Have varying quality.

# Nodes list

- `Highway`: yet another implementation but overkill version of pipe and reroute.
- `Junction`: over-the-head data packing and unpacking sequentially.

---

### Highway

<p align="center">
    <img src="https://raw.githubusercontent.com/Trung0246/ComfyUI-0246/main/assets/Screenshot%202023-11-05%20181932.png">
</p>

More complex example of what could be done (using my personal workflow as example) with extensions like `use-everywhere`:
<p align="center">
    <img src="https://raw.githubusercontent.com/Trung0246/ComfyUI-0246/main/assets/Screenshot%202023-11-06%20002520.png">
</p>

The query syntax goes as follow:

- `>name`: input variable.
- `<name`: output variable.
- `` >`n!ce n@me` ``: input variable but with special character and spaces (except `` ` ``, obviously).
- `!name`: output variable, but also delete itself, preventing from being referenced further.
  -  CURRENTLY BROKEN DUE TO HOW COMFYUI UPDATE THE NODES.
-  `<name1; >name2; !name3`: multiple input and outputs together.

For now `Highway` node is probably stable, as long as there's no cyclic connection.
  - Cyclic connection means that input and output of the same `Highway` node must not be connect, including indirect connection.
    - Else will be recursion error due to how ComfyUI execute nodes (trust me I tried).

Can probably have "nested Highway" but probably useless since the node have unlimited in-out pins.

Note for [chrisgoringe/cg-use-everywhere](https://github.com/chrisgoringe/cg-use-everywhere) users:
- Since inout pins are dynamic, therefore it is impossible to target `Highway` pins using `Anything Everywhere?` node, althrough the only exception is `_pipe_in` pin which is static, so that's the workaround for now.

Demo workflow is in [assets/workflow_highway.json](https://github.com/Trung0246/ComfyUI-0246/blob/main/assets/workflow_highway.json).

Special thanks to [@kijai](https://github.com/kijai/ComfyUI-KJNodes) for `ConditioningMultiCombine` node as which `Highway` node is based of.
   
##### TODO (may or may not get implemented)

- Cyclic detection in JS (python probably not possible unless I figure out a way how to extract the node graph).
- Node force update (for `!name`).

---

### Junction

<p align="center">
    <img src="https://raw.githubusercontent.com/Trung0246/ComfyUI-0246/main/assets/Screenshot%202023-11-07%20040534.png">
</p>

The offset syntax goes as follow:

- `type,1`: `type` is the type (usually `LATENT`, `MODEL`, `VAE`, etc.) and `1` is the index being set.
- `type,+2`: Same as above but instead of set offset, it increase the offset instead.
- `type,-2`: Decrease offset.
- `type1, -1; type2, +2; type3, 4`: Multiple offset.

Can automatically expand pins.

Inspired by /u/GianoBifronte ideas

##### TODO (may or may not get implemented)

- More robust test since I only test this with simple test cases.
- Same cyclic issue with `Highway`.
-  [`Mirror`](https://github.com/Trung0246/ComfyUI-0246/issues/4#issuecomment-1799448804), `Junction Plus`, `Merge` to merge `Highway` or `Junction` together, `Filter`, `Extract` to extract subset from `Highway` or `Junction`. `Dummy` to ignore and skip some output of `Junction` by connecting to it and do nothing else. `Probe` to debug internal data 
