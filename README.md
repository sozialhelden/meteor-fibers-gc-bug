# meteor-fibers-gc-bug

Reproduces a crash in Meteor / node-fibers because WASM-GC and node-fibers [is incompatible](https://github.com/nodejs/node/issues/29767#issuecomment-536505347).

## See also

- https://github.com/meteor/meteor/issues/11143
- https://github.com/meteor/meteor/issues/9568
- https://github.com/meteor/meteor/pull/10798
- https://github.com/connor4312/blake3/issues/12
- https://github.com/nodejs/node/issues/29767
- Broken workaround that can cause random out-of-memory crashes in containers: https://github.com/sandstorm-io/sandstorm/commit/13aed75ace593fbaf66ff3f5ac908441cf82b871
