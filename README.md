# adminify-workspace

**Private, internal-only.** A pnpm workspace of git submodules that pulls together every Adminify repo for cross-package development — the single-checkout experience the old Adminify monorepo had, kept alive now that each package/plugin/adapter lives in its own repo with its own history, issues, and CI.

This repo has no source of its own. Canonical code, issues, and releases live in each submodule's own repo:

- [`adminify`](https://github.com/ZakoutTech/adminify) — core framework (support, adapter, core, nestjs)
- [`adminify-design-system`](https://github.com/ZakoutTech/adminify-design-system)
- [`adminify-cli`](https://github.com/ZakoutTech/adminify-cli) (cli + schematics)
- [`adminify-typeorm`](https://github.com/ZakoutTech/adminify-typeorm) / [`adminify-mikroorm`](https://github.com/ZakoutTech/adminify-mikroorm) / [`adminify-prisma`](https://github.com/ZakoutTech/adminify-prisma) / [`adminify-in-memory`](https://github.com/ZakoutTech/adminify-in-memory)
- [`adminify-storage`](https://github.com/ZakoutTech/adminify-storage) (storage + media)
- [`adminify-audit-log`](https://github.com/ZakoutTech/adminify-audit-log)
- [`adminify-bullmq`](https://github.com/ZakoutTech/adminify-bullmq)
- [`wallet`](https://github.com/ZakoutTech/wallet) — standalone, not part of the Adminify family
- [`adminify-docs`](https://github.com/ZakoutTech/adminify-docs)
- [`adminify-examples`](https://github.com/ZakoutTech/adminify-examples)

## Clone

```bash
git clone --recurse-submodules git@github.com:ZakoutTech/adminify-workspace.git
```

Already cloned without `--recurse-submodules`?

```bash
git submodule update --init --recursive
```

## Working across packages

```bash
pnpm install
pnpm build   # turbo run build, across every submodule's packages
pnpm test    # turbo run test
pnpm lint    # turbo run lint
```

Each submodule is a normal git checkout of its own repo — commit, branch, and push from inside it exactly as you would in that repo cloned on its own; this repo only tracks *which commit* of each submodule the workspace currently points at. After changing which commit a submodule is on:

```bash
git add <submodule-path>
git commit -m "chore: bump <submodule-path>"
```

## Cross-repo dependencies

Until the core/adapter/plugin packages are published to npm, each split repo's own `package.json` still lists its Adminify dependencies as `workspace:*` (unresolvable standalone). Because this repo's `pnpm-workspace.yaml` lists every submodule's package directory as a workspace member, `workspace:*` resolves correctly *here* the same way it did in the original monorepo — this is the practical reason this repo exists.
