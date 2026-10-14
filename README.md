<!-- This should be the location of the title of the repository, normally the short name -->
# Key Protect PKCS#11 

<!-- Build Status, is a great thing to have at the top of your repository, it shows that you take your CI/CD as first class citizens -->
<!-- [![Build Status](https://travis-ci.org/jjasghar/ibm-cloud-cli.svg?branch=master)](https://travis-ci.org/jjasghar/ibm-cloud-cli) -->

<!-- Not always needed, but a scope helps the user understand in a short sentance like below, why this repo exists -->

<!-- A more detailed Usage or detailed explaination of the repository here -->
## Usage

This repository contains some example best practices for open source repositories:

* [LICENSE](LICENSE)
* [README.md](README.md)
* [CONTRIBUTING.md](CONTRIBUTING.md)
* [MAINTAINERS.md](MAINTAINERS.md)
* [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
<!-- A Changelog allows you to track major changes and things that happen, https://github.com/github-changelog-generator/github-changelog-generator can help automate the process -->
* [CHANGELOG.md](CHANGELOG.md)

# Overview

> [!WARNING]
> **Disclaimer:** The software provided here is for educational purposes only. Use it at your own risk.

IBM Cloud® Key Protect has a dedicated key management offering. This service allows you to take ownership of a cloud HSM to fully manage your encryption keys and to perform cryptographic operations.

# Installing the PKCS #11 files

The files contained in this repository allow clients to access a cloud HSM via the Key Protect service using a PKCS #11 library and its associated configuration file. The files are categorized by *releases*, which can be accessed from the `key-protect-pkcs11` repository's [releases URL](https://github.com/IBM-Cloud/key-protect-pkcs11/releases).

**NOTE:** The PKCS #11 library is currently supported only on Linux amd64(GLIBC distros only, so not compatible with Alpine for now) and Darwin arm64.

There are two files used along with your PKCS #11 application:

1. The PKCS #11 library:  pkcs11-grep11-**platform**.so.**major.minor.build** (Linux) or pkcs11-grep11-**platform**.**major.minor.build**.dylib (macOS)

   * **platform** is amd64 or arm64
  
   * **major.minor.build** refers to the version of the library

2. The PKCS #11 header files, included in the release alongside the library:

   * *pkcs11.h*: The main PKCS #11 header. It includes *pkcs11t.h* and *pkcs11f.h*.

   * *pkcs11t.h*: The standard PKCS #11 types and constants.

   * *pkcs11f.h*: The standard PKCS #11 function prototypes.

   * *libpkcs11-kms.h*: The header for the library's own declarations.

   * The headers are needed only when you build your own application against the PKCS #11 library. Add the directory that contains them to your compiler's include path (e.g., `-I<path-to-headers>`) and use `#include "pkcs11.h"`.

   * Applications that load the library at runtime, such as pkcs11-tool, do not need the header files.

## Verify the integrity and authenticity of the PKCS #11 library

For maximum security, you can optionally verify the integrity and authenticity of the PKCS #11 library. The library is published with a detached [GPG](https://gnupg.org/) signature. If the downloaded PKCS #11 library file is altered or corrupted, the signature check fails. To make sure the files are not tampered with or corrupted during the download process, complete the following steps by using the [GnuPG command-line tool](https://gnupg.org/download/).

1. Download the following files from the `key-protect-pkcs11` repository's [releases URL](https://github.com/IBM-Cloud/key-protect-pkcs11/releases) to the same directory where you store the PKCS #11 library:

    * `libpkcs11-kms.so.sig`: The detached signature of the PKCS #11 library. Use the signature file that matches the library file you downloaded.
      * only applies to the Linux

    * `libpubkey-pkcs11.asc`: The public key used to verify the signature.

2. Import the public key:

   `gpg --import libpubkey-pkcs11.asc`

3. Verify the integrity and authenticity of the PKCS #11 library file with the following command:

   `gpg --verify libpkcs11-kms.so.sig libpkcs11-kms.so`

   When the verification is successful, `Good signature from "International Business Machines Corporation <psirt@us.ibm.com>"` is displayed.

   **NOTE:** GPG also prints `WARNING: This key is not certified with a trusted signature!` unless you have marked the key as trusted. This is expected. Confirm that the key fingerprint in the output is `B640 FE80 0920 AD9A EB69 401C 6055 7144 A18B 07BE`.

4. If the verification fails, cancel the installation and contact [IBM for support](https://cloud.ibm.com/docs/key-protect?topic=key-protect-getting-help).

## Initializing the Keystores

Prior to using the PKCS #11 library, the keystores must be initialized. Please see [Managing a keystore](https://cloud.ibm.com/docs/key-protect?topic=key-protect-pkcs11-manage-keystore&interface=ui)

Once the keystores have been initialized, normal and anonymous users can proceed with key operations such as `C_GenerateKey` or `C_GenerateKeyPair`.

## License

All source files must include a Copyright and License header. The SPDX license header is
preferred because it can be easily scanned.

If you would like to see the detailed LICENSE click [here](LICENSE).

```text
#
# Copyright IBM Corp. 2026 - 2026
# SPDX-License-Identifier: Apache-2.0
#
```

## Authors

Optionally, you may include a list of authors, though this is redundant with the built-in
GitHub list of contributors.

* Author: Timothy Yao <Timothy.Yao@ibm.com>
