# AGENTS.md — layer-kubevirt

Standalone candy repo for the `kubevirt` layer — installs the pinned **virtctl**
KubeVirt CLI binary at `/usr/bin/virtctl`. virtctl drives a KubeVirt cluster from
the command line (start/stop/restart a VirtualMachine, serial console or VNC,
ssh/port-forward into a guest, snapshot, live-migrate). The candy lives in
`charly.yml` at the repo root. It carries **no `skill:` entity**, so no owning
`/charly-<family>:<name>` skill is projected into the marketplace corpus.

Canonical files:

- `charly.yml` — the `kubevirt:` candy entity (no `skill:` entity present).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-kubernetes:kubernetes` — the closest owning skill for the KubeVirt
  platform surface this client serves.
- `/charly-internals:plugin` — the plugin authoring reference; the consumer
  `candy/plugin-kubevirt` shells out to `virtctl` for the `charly kubevirt
  console/ssh/port-forward` verbs.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `download:`/`check:`). Load before editing any entity
  field or plan step.
- `/charly-internals:git-workflow` — before any git/PR action.

There is no dedicated `/charly-*:layer-kubevirt` owning skill — this repo's candy
carries no `skill:` entity. The gap is recorded against
`opencharly/opencharly#291`; when one is authored, add it here.

## Build / validate / test

- The candy's `plan:` steps are the functional evidence: the `download:` step
  installs the pinned `virtctl` release binary for the target arch (the
  auto-exported `TARGETARCH`), and the two `check:` steps assert the binary exists
  at `/usr/bin/virtctl` and that `virtctl version --client` exits 0 with `Client
  Version` on stdout.
- `charly box validate` at the repo root — the structural check (the candy + the
  `download:`/`check:` steps).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate and ships only `.github/workflows/tag-on-merge.yml`.

## Modify this repo

- Edit the `kubevirt:` candy entity in `charly.yml`.
- Bump `KUBEVIRT_VERSION` in the candy's `var:` block to move the pinned
  `virtctl` release; the download URL derives from it and the asset name uses the
  KubeVirt release naming (`virtctl-<version>-linux-<arch>`).
- Keep the install idempotent (`unless_exists: /usr/bin/virtctl`) and the binary
  at the stable `/usr/bin/virtctl` path the consumer expects.

## Landing

Every change lands through a pull request gated by the org-required
`charly/pr-validator`. The landing mechanics — the `feat/` branch, the PR-only
rule, `CHANGELOG/` history, and the tag-on-merge CalVer — are owned by
`/charly-internals:git-workflow` and the umbrella `AGENTS.md` /
`charly/AGENTS.md`; this signpost points at them and does not restate them.
