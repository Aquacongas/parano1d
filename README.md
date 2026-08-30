## ParanO1c by Aquacongas

**ParanO1c** is an experimental community-developed Android implementation of
Parano1d, maintained by **Aquacongas**.

Its purpose is to bring the Parano1d wallet and node experience to Android
without creating a separate protocol, network or cryptocurrency.

ParanO1c follows the upstream Parano1d consensus rules, transaction format,
wallet derivation, authorization model, State model, mempool rules and P2P
protocol. The Android application connects these shared protocol components to
a dedicated mobile runtime and Android user interface.

### Relationship to the native Parano1d GUI wallet

Conceptually, ParanO1c is the Android counterpart of the native Parano1d GUI
wallet.

The desktop GUI supervises a local Parano1d node and exposes wallet and node
functionality through a graphical interface. ParanO1c provides the same kind of
user-facing wallet/node experience on Android, but uses an Android-specific
application layer and mobile runtime instead of the desktop GUI integration.

Secret-bearing wallet logic is shared through the Parano1d wallet core rather
than being reimplemented in Kotlin. The Android UI communicates with the Rust
implementation through a dedicated FFI layer.

This means ParanO1c is not a protocol reimplementation from scratch and does
not maintain an independent wallet cryptography design. Where practical, it
reuses the same Parano1d chain, transaction, wallet, mempool and verification
logic used by the normal node.

Android-specific code exists where the platform requires a different runtime,
storage integration, synchronization path, networking integration or user
interface.

ParanO1c currently contains no cryptocurrency mining functionality.

### Downstream maintenance model

ParanO1c is maintained as a downstream fork of the official Parano1d
repository.

The Parano1d project does not currently support Android as an official core
target. For that reason, Android-specific application code, mobile crates, FFI
code and Android compatibility changes are maintained in this repository
rather than merged into the upstream Parano1d core repository.

Upstream Parano1d remains the protocol source of truth.

For each upstream release, ParanO1c development follows this model:

1. Fetch the latest upstream Parano1d changes.
2. Merge or rebase the ParanO1c branch onto the selected upstream release.
3. Preserve the downstream Android/mobile commits.
4. Resolve only genuine conflicts caused by upstream changes touching the same
   code.
5. Rebuild and test the Android node, wallet, synchronization, transaction
   submission and network compatibility.
6. Publish a new immutable ParanO1c source tag and matching Android release.

Git therefore preserves the Android-specific downstream changes across normal
upstream updates. Separate patch scripts are not the primary maintenance
mechanism; the Android changes remain normal commits in the fork.

The upstream repository is:

```text
https://github.com/ignotusnemo/parano1d
```

The ParanO1c downstream repository is:

```text
https://github.com/Aquacongas/parano1d
```

A typical upstream update is performed with:

```sh
git fetch upstream
git rebase upstream/main
```

or, when a merge-based workflow is preferred:

```sh
git fetch upstream
git merge upstream/main
```

If upstream changes do not conflict with the Android-specific commits, Git
carries the ParanO1c changes forward automatically.

If upstream and ParanO1c both modify the same code, Git stops at the conflict
and the affected downstream code is reviewed and resolved before a new Android
release is produced.

### Android-specific implementation

The mobile implementation is primarily contained in dedicated crates including:

```text
noid_mobile_node
noid_mobile_wallet
noid_mobile_ffi
noid_mobile_networking
noid_mobile_sync_apply
noid_mobile_history_runtime
```

The Android application itself is located under:

```text
android/
```

The mobile runtime uses the shared Parano1d wallet core.

The Android UI does not implement wallet cryptography independently. Wallet and
secret-bearing operations remain in Rust and are exposed to the Android
application through the mobile FFI layer.

The mobile wallet uses the same master-secret derivation and shared wallet
logic used by Parano1d rather than introducing a separate Android key hierarchy.

On Android, the local wallet key is stored inside the application's private data
directory as:

```text
wallet.key
```

using the current shared Parano1d wallet keystore format.

The current shared wallet format stores the master secret without
password-based encryption. On Android the file is protected by the
application's private sandbox and is not intentionally written to Downloads,
shared external storage or other public application-accessible locations.

The master key can be explicitly exported by the user from the Android
interface. Export displays the key first and copying it to the Android clipboard
requires a separate explicit user action.

Clipboard use is therefore optional and user-initiated rather than an automatic
wallet operation.

The Android implementation also carries a small number of platform-specific
compatibility changes required to run the shared Parano1d code on Android.

These changes are maintained downstream and are kept as isolated as practical
so that future upstream releases remain straightforward to integrate.

### Relationship to upstream development

ParanO1c does not attempt to redefine or independently modify the Parano1d
protocol.

Protocol and consensus development continues upstream in Parano1d.

When a new upstream version is released, the ParanO1c fork is updated against
that release and the Android-specific layer is rebuilt and retested on top of
the new upstream code.

This means ParanO1c can continue to follow Parano1d development while keeping
Android support independent from the security-critical upstream core.

The goal is to keep the downstream difference as small, isolated and
understandable as possible.

### Release model

Every ParanO1c APK release should correspond to an immutable source tag.

A release should identify:

- the exact ParanO1c source tag;
- the exact ParanO1c source commit;
- the upstream Parano1d version;
- the exact upstream Parano1d base commit;
- the APK SHA-256;
- the signing-certificate SHA-256 fingerprint;
- the dependencies and build procedure required to reproduce the build.

The current Android v1.0.4 release uses:

```text
ParanO1c tag:
android-v1.0.4-final

ParanO1c source commit:
375238535c69ac1f2f20df2111af8be346954987

Upstream Parano1d version:
v1.0.4

Upstream base commit:
fedbe6e3c0ddf8b8372546017bb9bc341acb8ab0
```

The corresponding release is available at:

```text
https://github.com/Aquacongas/parano1d/releases/tag/android-v1.0.4-final
```

### Security

ParanO1c is an experimental community-maintained project.

It is **not an official Parano1d release**, is not endorsed as an official
Parano1d wallet, and has not undergone a completed independent security audit.

Security-sensitive ParanO1c vulnerabilities should be reported privately rather
than through public issues.

GitHub Private Vulnerability Reporting is enabled for the repository:

```text
https://github.com/Aquacongas/parano1d
```

Use:

```text
Security -> Advisories -> Report a vulnerability
```

for issues that may expose wallet secrets, allow unauthorized spending, affect
wallet storage or otherwise place user funds at risk.

The repository also provides a dedicated security policy in:

```text
SECURITY.md
```

Until additional independent review has been completed, ParanO1c should be
treated as experimental software and users should use appropriately small
amounts.
