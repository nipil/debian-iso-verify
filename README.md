# debian-iso-verify

Secure fetch and verify of debian iso images

    Usage: ./debian-iso-verify.sh [-a ARCH] [-s SUMS] [-t TARGET] [-c MIRROR_DNS] [FOLDER]

    -a allows overriding the default amd64 architecture
    -s allows overriding the default SHA256SUMS hashes
    -t allows overriding the default iso-cd target
    -c allows downloading from specified mirror (DNS entry, not url) before verifying it

The content of the FOLDER will be verified : hash signature, and the hashes.

The keys used for verification are embedded in the script :

- to allow for standalone use of the script
- to anchor the trust as keys are fetched from out of band

In the simplest case, all you have to do is run the script using -c your_favorite_mirror in a cron.

Files are fetched using curl, which allows for using environment variables for proxy settings.

Example of online fetch and verify, using an HTTP proxy (here, tested in devcontainer) :

    $ export http_proxy=squid:3128
    $ ./debian-iso-verify.sh -c ftp.crifo.org
    Using mirror ftp.crifo.org to download current version
    Artefacts stored into folder 12.11.0
    ... (verification shown below)

Example of an offline verification :

    $ ./debian-iso-verify.sh 12.11.0
    Verifying signature of hash file...
    gpg: Signature made Sat 17 May 2025 05:55:58 PM UTC
    gpg:                using RSA key DF9B9C49EAA9298432589D76DA87E80D6294BE9B
    gpg: Good signature from "Debian CD signing key <debian-cd@lists.debian.org>" [unknown]
    gpg: WARNING: This key is not certified with a trusted signature!
    gpg:          There is no indication that the signature belongs to the owner.
    Primary key fingerprint: DF9B 9C49 EAA9 2984 3258  9D76 DA87 E80D 6294 BE9B
    Verifying hash of available files...
    debian-12.11.0-amd64-netinst.iso: OK
    Verification of 12.11.0 completed
