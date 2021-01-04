# meteor-fibers-gc-bug

Reproduces a crash in Meteor / node-fibers because WASM-GC and node-fibers [is incompatible](https://github.com/nodejs/node/issues/29767#issuecomment-536505347).

## See also

- https://github.com/meteor/meteor/issues/11143
- https://github.com/meteor/meteor/issues/9568
- https://github.com/meteor/meteor/pull/10798
- https://github.com/connor4312/blake3/issues/12
- https://github.com/nodejs/node/issues/29767
- Broken workaround that can cause random out-of-memory crashes in containers: https://github.com/sandstorm-io/sandstorm/commit/13aed75ace593fbaf66ff3f5ac908441cf82b871

## Reproduction

```bash
git clone git@github.com:sozialhelden/meteor-fibers-gc-bug.git
cd meteor-fibers-gc-bug
meteor npm install
meteor
```

Output:
```
[[[[[ ~/meteor-fibers-gc-bug ]]]]]

=> Started proxy.
I20210104-15:38:42.701(1)? Greetings from /server/main.js!
W20210104-15:38:42.722(1)? (STDERR)
W20210104-15:38:42.722(1)? (STDERR)
W20210104-15:38:42.722(1)? (STDERR) #
W20210104-15:38:42.722(1)? (STDERR) # Fatal error in , line 0
W20210104-15:38:42.723(1)? (STDERR) # archived threads in combination with wasm not supported
W20210104-15:38:42.723(1)? (STDERR) #
W20210104-15:38:42.723(1)? (STDERR) #
W20210104-15:38:42.723(1)? (STDERR) #
W20210104-15:38:42.723(1)? (STDERR) #FailureMessage Object: 0x7ffeefbf4670
W20210104-15:38:42.723(1)? (STDERR)  1: 0x10010bdc2 node::NodePlatform::GetStackTracePrinter()::$_3::__invoke() [/Users/hypo2/.meteor/packages/meteor-tool/.1.12.0.c4gh2d.5993u++os.osx.x86_64+web.browser+web.browser.legacy+web.cordova/mt-os.osx.x86_64/dev_bundle/bin/node]
W20210104-15:38:42.723(1)? (STDERR)  2: 0x100f5c152 V8_Fatal(char const*, ...) [/Users/hypo2/.meteor/packages/meteor-tool/.1.12.0.c4gh2d.5993u++os.osx.x86_64+web.browser+web.browser.legacy+web.cordova/mt-os.osx.x86_64/dev_bundle/bin/node]
W20210104-15:38:42.723(1)? (STDERR)  3: 0x1007b73b2 v8::internal::wasm::(anonymous namespace)::CheckNoArchivedThreads(v8::internal::Isolate*)::ArchivedThreadsVisitor::VisitThread(v8::internal::Isolate*, v8::internal::ThreadLocalTop*) [/Users/hypo2/.meteor/packages/meteor-tool/.1.12.0.c4gh2d.5993u++os.osx.x86_64+web.browser+web.browser.legacy+web.cordova/mt-os.osx.x86_64/dev_bundle/bin/node]
W20210104-15:38:42.724(1)? (STDERR)  4: 0x10032d5eb v8::internal::ThreadManager::IterateArchivedThreads(v8::internal::ThreadVisitor*) [/Users/hypo2/.meteor/packages/meteor-tool/.1.12.0.c4gh2d.5993u++os.osx.x86_64+web.browser+web.browser.legacy+web.cordova/mt-os.osx.x86_64/dev_bundle/bin/node]
W20210104-15:38:42.724(1)? (STDERR)  5: 0x1007b5d20 v8::internal::wasm::WasmEngine::ReportLiveCodeFromStackForGC(v8::internal::Isolate*) [/Users/hypo2/.meteor/packages/meteor-tool/.1.12.0.c4gh2d.5993u++os.osx.x86_64+web.browser+web.browser.legacy+web.cordova/mt-os.osx.x86_64/dev_bundle/bin/node]
W20210104-15:38:42.724(1)? (STDERR)  6: 0x10032c589 v8::internal::StackGuard::HandleInterrupts() [/Users/hypo2/.meteor/packages/meteor-tool/.1.12.0.c4gh2d.5993u++os.osx.x86_64+web.browser+web.browser.legacy+web.cordova/mt-os.osx.x86_64/dev_bundle/bin/node]
W20210104-15:38:42.724(1)? (STDERR)  7: 0x100692f2c v8::internal::Runtime_StackGuard(int, unsigned long*, v8::internal::Isolate*) [/Users/hypo2/.meteor/packages/meteor-tool/.1.12.0.c4gh2d.5993u++os.osx.x86_64+web.browser+web.browser.legacy+web.cordova/mt-os.osx.x86_64/dev_bundle/bin/node]
W20210104-15:38:42.724(1)? (STDERR)  8: 0x1009dcbd9 Builtins_CEntry_Return1_DontSaveFPRegs_ArgvOnStack_NoBuiltinExit [/Users/hypo2/.meteor/packages/meteor-tool/.1.12.0.c4gh2d.5993u++os.osx.x86_64+web.browser+web.browser.legacy+web.cordova/mt-os.osx.x86_64/dev_bundle/bin/node]
=> Exited from signal: SIGILL
=> Your application is crashing. Waiting for file change.
```
