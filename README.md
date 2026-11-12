# resources

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Apple Push Services](https://img.shields.io/badge/Apple%20Push%20Services-certificate-000000)](Certificate/certificate.p12)
[![Team ID](https://img.shields.io/badge/Team%20ID-F474LTNP59-555555)](Certificate/TeamId.txt)

This repository holds the Apple Push Services material for the iOS bundle identifier `com.aryansbtloe.apns-tester.sample.ios`, under Apple Developer Team `F474LTNP59` (Alok Singh, IN). `Certificate/certificate.p12` is a PKCS#12 bundle whose password [`Certificate/README.txt`](Certificate/README.txt) records as empty. It contains the push certificate and a matching private key. Beside it are a P-256 PKCS#8 private key named for Key ID `RSHBA9D4N9`, plain-text copies of that Key ID and the Team ID, and a macOS Finder `.DS_Store` file. The current push certificate was issued by Apple Worldwide Developer Relations Certification Authority (OU G4) and is valid from 4 January 2026 through 3 February 2027.

## What's inside

### Apple Push Services certificate

- [`Certificate/certificate.p12`](Certificate/certificate.p12) — PKCS#12 (3,593 bytes). Friendly name `Apple Push Services: com.aryansbtloe.apns-tester.sample.ios`. It holds one certificate bag and one shrouded private-key bag. [`Certificate/README.txt`](Certificate/README.txt) says the password is empty, and the file opens with an empty password. The certificate bag is encrypted with `pbeWithSHA1And40BitRC2-CBC` (2,048 iterations) and the key bag with `pbeWithSHA1And3-KeyTripleDES-CBC` (2,048 iterations).

The certificate inside that bundle:

| Field | Value |
| --- | --- |
| Subject | `UID = com.aryansbtloe.apns-tester.sample.ios`, `CN = Apple Push Services: com.aryansbtloe.apns-tester.sample.ios`, `OU = F474LTNP59`, `O = Alok Singh`, `C = IN` |
| Issuer | `CN = Apple Worldwide Developer Relations Certification Authority`, `OU = G4`, `O = Apple Inc.`, `C = US` |
| Validity | 4 January 2026 07:49:05 GMT – 3 February 2027 07:49:04 GMT |
| Serial | `203F0E7BA833CADA145277BFB6EAF703` |
| SHA-256 fingerprint | `45:DC:CD:3E:F8:7A:44:31:4B:5D:83:C3:DB:90:3D:63:D7:A7:8E:D8:19:BD:08:24:68:DF:DC:56:7C:93:B3:4A` |
| Public key | RSA 2048-bit (`sha256WithRSAEncryption`) |
| Key usage | Digital Signature (critical) |
| Extended key usage | TLS Web Client Authentication |
| Basic constraints | CA:FALSE (critical) |
| Subject key identifier | `90:CE:4B:59:69:7F:D3:59:A0:36:B4:DF:7B:52:08:06:3E:01:D3:3C` (this is also the PKCS#12 `localKeyID`) |
| Certificate policy | `1.2.840.113635.100.5.1` |

Apple push extension `1.2.840.113635.100.6.3.6` lists these topic strings:

- `com.aryansbtloe.apns-tester.sample.ios` (`topic`)
- `com.aryansbtloe.apns-tester.sample.ios.voip` (`voip`)
- `com.aryansbtloe.apns-tester.sample.ios.complication` (`complication`)
- `com.aryansbtloe.apns-tester.sample.ios.voip-ptt` (`voip-ptt`)

The same certificate also carries Apple extensions `1.2.840.113635.100.6.3.1` and `1.2.840.113635.100.6.3.2`. Authority information and revocation pointers embedded in the certificate are `http://certs.apple.com/wwdrg4.der`, `http://ocsp.apple.com/ocsp03-wwdrg4`, `http://crl.apple.com/wwdrg4-4.crl`, and `https://www.apple.com/certificateauthority`.

### PKCS#8 private key

- [`Certificate/AuthKey_RSHBA9D4N9.p8`](Certificate/AuthKey_RSHBA9D4N9.p8) — 257-byte PEM file. The header is `-----BEGIN PRIVATE KEY-----`. It is an unencrypted PKCS#8 private key on the P-256 curve (`prime256v1`). The filename is `AuthKey_` plus the value in [`Certificate/KeyId.txt`](Certificate/KeyId.txt). The repository does not record which Apple service this key signs for.

### Identifiers and the original note

- [`Certificate/README.txt`](Certificate/README.txt) — the only usage note that shipped with these files. Its text is: "Password for certificate.p12 is empty".
- [`Certificate/KeyId.txt`](Certificate/KeyId.txt) — `RSHBA9D4N9`.
- [`Certificate/TeamId.txt`](Certificate/TeamId.txt) — `F474LTNP59`. This is the same team identifier as the certificate subject `OU`.

### Finder metadata

- [`Certificate/.DS_Store`](Certificate/.DS_Store) — 6,148-byte Apple Desktop Services Store file (macOS Finder metadata), committed with the certificate files. It has no documented role in using the certificate.

## How the files relate

```mermaid
flowchart TD
  repo["resources"]
  certDir["Certificate/"]
  repo --> certDir
  certDir --> readme["README.txt"]
  certDir --> keyId["KeyId.txt"]
  certDir --> teamId["TeamId.txt"]
  certDir --> p8["AuthKey_RSHBA9D4N9.p8"]
  certDir --> p12["certificate.p12"]
  certDir --> ds[".DS_Store"]
  p12 --> pushCert["Apple Push Services certificate"]
  p12 --> keyBag["Shrouded private key"]
  readme -->|"password is empty"| p12
  keyId -->|"RSHBA9D4N9"| p8
  teamId -->|"F474LTNP59"| pushCert
```

## How to use

There is no application, package manifest, build, or test suite in this repository. The files are the credential drop itself.

1. Read the Team ID from [`Certificate/TeamId.txt`](Certificate/TeamId.txt) and the Key ID from [`Certificate/KeyId.txt`](Certificate/KeyId.txt). The `.p8` filename is `AuthKey_` followed by that Key ID.
2. [`Certificate/README.txt`](Certificate/README.txt) states that the password for `certificate.p12` is empty. OpenSSL 3 needs `-legacy` because the bundle uses the RC2 and 3DES algorithms named above. This prints the certificate subject, issuer, and validity, and does not write the private key out:

```bash
openssl pkcs12 -in Certificate/certificate.p12 -nokeys -passin pass: -legacy -clcerts \
  | openssl x509 -noout -subject -issuer -dates
```

`AuthKey_RSHBA9D4N9.p8` is an unencrypted private key, and the PKCS#12 password recorded in the repository is empty. Treat both files as secrets.

## Project structure

```text
.
├── LICENSE
├── README.md
└── Certificate/
    ├── .DS_Store
    ├── AuthKey_RSHBA9D4N9.p8
    ├── KeyId.txt
    ├── README.txt
    ├── TeamId.txt
    └── certificate.p12
```

History recorded in git:

- `42a3f79` — "Please check README.txt" (15 August 2020, Alok kumar singh). Added the `Certificate/` directory. The `certificate.p12` blob in that commit is an Apple Push Services certificate for the same subject, valid from 7 December 2021 to 6 January 2023, issued by Apple Worldwide Developer Relations Certification Authority.
- `0479084` — "Updated certificate to latest" (4 January 2026, Alok Singh). Replaced `certificate.p12` (3,441 bytes to 3,593 bytes). That is the certificate described in the table above, issued by Apple Worldwide Developer Relations Certification Authority, OU G4.

Both commits use the email `alok.singh.confident@gmail.com`.

## Contributing

The default branch is `master`. There is no separate contributing guide, code of conduct, CI workflow, or test command, because the repository is this certificate snapshot.

Open an issue or a pull request against `master` for a correction to the notes. Leave the Apple-issued certificate, the private keys, and `.DS_Store` as they are unless you are replacing the credential drop itself. Do not paste private-key material into issues or pull requests.

## License

The [MIT License](LICENSE), copyright 2026 Alok Singh, covers Alok Singh's own documentation in this repository: this README, [`Certificate/README.txt`](Certificate/README.txt), [`Certificate/KeyId.txt`](Certificate/KeyId.txt), and [`Certificate/TeamId.txt`](Certificate/TeamId.txt).

[`Certificate/certificate.p12`](Certificate/certificate.p12) contains a certificate issued by Apple Inc. (Apple Worldwide Developer Relations Certification Authority). The certificate states: "Reliance on this certificate by any party assumes acceptance of the then applicable standard terms and conditions of use, certificate policy and certification practice statements." Those Apple terms stay in force for that certificate. See `https://www.apple.com/certificateauthority`, which is the policy URL embedded in the certificate. The private key stored in the same PKCS#12 file is not covered by the MIT license.

[`Certificate/AuthKey_RSHBA9D4N9.p8`](Certificate/AuthKey_RSHBA9D4N9.p8) is a private key and is not covered by the MIT license. [`Certificate/.DS_Store`](Certificate/.DS_Store) is macOS Finder metadata and is not covered by the MIT license.

## Author

Alok Singh — [github.com/aryansbtloe](https://github.com/aryansbtloe) — [bestlatestapps.com](https://bestlatestapps.com)
