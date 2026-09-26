# layer-kubevirt

The `layer-kubevirt` candy of the [opencharly/charly](https://github.com/opencharly/charly)
candy library, as a standalone repo (the candy de-submodule cutover, kind-prefixed
naming). It installs the pinned **virtctl** KubeVirt CLI binary at `/usr/bin/virtctl`.

The candy manifest lives at the repo root; the charly resolver fetches this repo at
the pinned tag. `candy/plugin-kubevirt` shells out to `virtctl` for the
`charly kubevirt console/ssh/port-forward` verbs.
