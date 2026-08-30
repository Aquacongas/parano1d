# Security Policy

## ParanO1c Security

ParanO1c is an experimental community-developed Android implementation based on Parano1d.

It is maintained independently by Aquacongas and is not an official Parano1d release.

ParanO1c has not undergone a completed independent security audit.

Security-sensitive issues should be reported privately before any public disclosure.

## Reporting a Vulnerability

If you discover a vulnerability that may affect wallet security, user funds, master keys, transaction authorization, wallet storage, Android integration, FFI, synchronization, mempool interaction or node integration, please do not open a public GitHub Issue and do not post technical exploitation details publicly.

Please use GitHub Private Vulnerability Reporting for this repository when available.

Repository:

https://github.com/Aquacongas/parano1d

Go to:

Security -> Advisories -> Report a vulnerability

If GitHub Private Vulnerability Reporting is temporarily unavailable, contact the ParanO1c developer privately through GitHub or Discord.

Security-sensitive reports may also be escalated through a Parano1d Community Admin if necessary.

## What Should Be Reported Privately

Examples include:

- master-key or spend-secret disclosure;
- unauthorized spending;
- incorrect transaction authorization;
- bypass of wallet security checks;
- remote code execution;
- malicious APK or update-related security issues;
- Android application sandbox or wallet-storage issues;
- unintended exposure of wallet secrets;
- vulnerabilities in the Rust/Kotlin FFI boundary;
- consensus or transaction-validation differences introduced by the Android implementation;
- malicious or invalid network data being accepted by the mobile node;
- vulnerabilities that can deterministically corrupt wallet state or cause loss of funds.

Ordinary UI bugs, crashes without a security impact, feature requests and non-sensitive testing feedback may be reported through normal GitHub Issues or community testing channels.

## Information to Include

Please include as much of the following information as possible:

- ParanO1c version;
- source commit or release tag;
- Android version;
- device or emulator model;
- clear description of the vulnerability;
- steps required to reproduce it;
- expected behaviour;
- actual behaviour;
- logs or screenshots with all wallet secrets removed;
- proof-of-concept code if required to demonstrate the issue;
- assessment of the possible security impact.

Do not include real master keys, private keys, recovery material, wallet backups or other secrets in a report.

If a proof of concept requires a wallet secret, use a newly created disposable test wallet.

## Coordinated Disclosure

Please allow reasonable time to investigate, reproduce and fix a vulnerability before publishing technical details.

If the issue is confirmed, I will attempt to:

1. reproduce and assess the vulnerability;
2. prepare a fix;
3. test the fix;
4. publish an updated release where required;
5. disclose the issue publicly after affected users have had a reasonable opportunity to update.

No specific response or remediation time can currently be guaranteed because ParanO1c is a community-maintained project.

## Supported Version

The primary supported version is the latest ParanO1c Android release published at:

https://github.com/Aquacongas/parano1d/releases

Security reports concerning older versions are still welcome, particularly when the same issue may affect the current release.

## Wallet Safety

ParanO1c should currently be treated as experimental software.

Users should avoid storing significant funds in the wallet until additional independent security review has been completed.

The current Android wallet uses the shared Parano1d wallet core.

The master key is stored in the wallet.key file inside the Android application's private data directory.

The current shared wallet.key format is not password-encrypted at rest.

Master-key export is explicitly initiated by the user. The wallet displays the master key first, and copying it to the Android clipboard requires a separate explicit COPY action.

Never send wallet secrets to developers, moderators or support staff.

The ParanO1c developer, Parano1d developers, Community Admins and moderators should never need your master key in order to investigate a bug.

## Scope

This security policy applies specifically to ParanO1c Android-specific code and downstream modifications maintained in:

https://github.com/Aquacongas/parano1d

Issues that are proven to exist unchanged in the upstream Parano1d implementation should also be reported according to the upstream Parano1d security policy:

https://github.com/ignotusnemo/parano1d/security

Thank you for helping improve ParanO1c security.
