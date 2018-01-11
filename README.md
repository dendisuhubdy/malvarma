# Malvarma: Secure Monero cold wallets for the truly paranoid
 
Malvarma aims to make Monero cold wallet generation easy and secure.  It caters
to highly security-conscious people, but is designed to be easy for an average
user to use. 

**Use at your own risk. Malvarma is in an alpha release stage and has not been vetted.**

This guide is for Ubuntu Linux users. 

## 1. Download the Malvarma image

Use any of these links:

[Mega](https://mega.nz/#!bpNQASSb!2mVpUW1cIB_35VHkGEPzCRcdi94EWoiew4_ZTkOI8n0)

[Dropbox](https://www.dropbox.com/s/496mpyrmzkx7qea/malvarma-0.1-alpha.tar.bz2?dl=0)

P2P Mirrors:

[Instant.io](https://instant.io/#1a600dce530029378851e80af65021edc5eb78ab)

BitTorrent Magnet URI:
```
magnet:?xt=urn:btih:1a600dce530029378851e80af65021edc5eb78ab&dn=malvarma-0.1-alpha.tar.bz2&tr=udp%3A%2F%2Fexplodie.org%3A6969&tr=udp%3A%2F%2Ftracker.coppersurfer.tk%3A6969&tr=udp%3A%2F%2Ftracker.empire-js.us%3A1337&tr=udp%3A%2F%2Ftracker.leechers-paradise.org%3A6969&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337&tr=wss%3A%2F%2Ftracker.btorrent.xyz&tr=wss%3A%2F%2Ftracker.fastcast.nz&tr=wss%3A%2F%2Ftracker.openwebtorrent.com
```

### 2. Unpack and verify the image

```bash
tar xf malvarma-0.1-alpha.tar.bz2 && \
cd malvarma-0.1-alpha && \
gpg --keyserver pgp.mit.edu --recv-keys 0x90DB43617CCC1632 && \
gpg --verify malvarma-0.1-alpha.img.sig malvarma-0.1-alpha.img && \
sha256sum -c malvarma-0.1-alpha.img.sha256
```

**Do not continue** if the SHA256 or GPG verification fails.

The following command burn the image to a microSD card assumes that the microSD
device is at `/dev/mmcblk0`:

```bash
sudo dd bs=4M if=malvarma-0.1-alpha.img of=/dev/mmcblk0 conv=fsync
```

Alternatively, use [Etcher](https://etcher.io/) to burn the image..

### 3. Boot the Raspberry Pi Zero

Insert the card into a non-wireless
[Raspberry Pi Zero](https://www.raspberrypi.org/products/raspberry-pi-zero/),
plug it into a HDMI screen and USB power source, and turn it on.

After 1-2 minutes, a mnemonic seed, private spend/view keys, and public address
should automatically show up on the screen. Write down all this information and
store it safely. **At minimum, write down the mnemonic seed.**

 Optional: following this visual guide, destroy particular components of the
Raspberry Pi Zero using hand tools. Specifically, destroy the microSD card and
the Raspberry Pi's BCM2835 processor. According to
[Adafruit](https://cdn-learn.adafruit.com/downloads/pdf/introducing-the-raspberry-pi-zero.pdf),
the Pi Zero's RAM sits on top of its processor, so be sure to destroy both.
Wear eye protection and use a hammer and screwdriver or nail to break through
their plastic coatings and pulverise their silicon innards.
   
For inspiration, refer to this article from The Guardian: 
[Footage released of Guardian editors destroying Snowden hard drives
](https://www.theguardian.com/uk-news/2014/jan/31/footage-released-guardian-editors-snowden-hard-drives-gchq)

<img src="https://raw.githubusercontent.com/weijiekoh/malvarma/master/destroy.png" height=250 />

While this is a little extreme, the device is inexpensive and small, and some
users may wish to maximise their peace of mind. If you don't want to destroy
the device, at least store it in a safe place and do not use it until you you
empty your cold wallet of all funds and no longer use that address.

## What is a cold wallet?

If you wish to store moneroj [1] for a long period of time without spending
them, it is a good idea to put them in a cold wallet, also known as [cold
storage](https://www.investopedia.com/terms/c/cold-storage.asp). Cold wallets
are created and stored in a such a way that the only way to spend the funds in
them is to have physical access.  As such, they should only be kept offline,
and should be generated by devices which have never and will never be connected
to the Internet.

## Why not use some other tool?

There is nothing intrinsically wrong with existing solutions, such as
[moneromooo's offline wallet
generator](https://github.com/moneromooo-monero/monero-wallet-generator). It is
absolutely possible to use such tools securely. Malvarma only seeks to go
one step further to provide a solution which accounts for *both* hardware and
software security, while making the entire process as foolproof as possible.

## Why use Malvarma?

Unfortunately, it is not easy to create a cold wallet securely. Several
problems stand in the way, so Malvarma seeks to mitigate them.

### Network interfaces

**Problem:** A cold wallet should be generated using processes and
devices which have as few attack surfaces as possible. Network connectivity is one
such attack surface though which an attacker may (hypothetically) leverage to
extract private keys.

**Solution:** 
A cold wallet should be generated in an
[air-gapped](https://en.wikipedia.org/wiki/Air_gap_(networking)) device. While
it is possible to disable wireless adapters on most PCs or laptops, the
non-wireless version of the Raspberry Pi Zero is *always* air-gapped, so it
avoids this entire class of vulnerabilities.

Note that this only matters if you don't want to have to *trust* that your device
can indeed turn its wireless adapter off. Again, this is a guide for the highly
security-conscious, and assumes an extremely low risk appitite.

### Malware

**Problem:** A device may be infected with malware without the user knowing.
Even if a cold wallet is generated offline, sufficiently sophisticated malware
may transmit it to an adversary if the device gets online.

**Solution:** A legitimate copy of Malvarma will not contain malware, and it
will be possible to ensure that this is so. Each Malvarma image will be
cryptographically signed and users should verify it before use. More advanced
users will be able to inspect, reproduce, and verify that the published
Malvarma image is exactly as advertised.

### Data destruction

**Problem:** Traces of private keys may remain on one's device even if they are
deleted or after it is shut down. [2] [3] The only way to be absolutely sure is
device to generate your keys and physically destroy the device after use.
Although this is prudent, it is wasteful to destroy electronics, and disposing
electronic waste can harm the environment. 

**Solution:** A Raspberry Pi Zero only costs USD$5 and is very small. It is
therefore more economical and relatively less harmful to the environment to
destroy after use.


### Ease of use

**Problem:** It is not trivial to set up a secure computing environment.
Rather than go through the hassle of setting up a USB-bootable Linux
distribution on an airgapped device just to create a cold wallet, a
less-determined user may opt to do so on the system they regularly use, and
thereby defeat the purpose of the entire exercise.

**Solution:** Each Raspberry Pi Zero is the same as the next. By encapsulating
a wallet generator into a microSD image, Malvarma eliminates the problem of
installing software on an airgapped device. The goal is to make it plug-and-play.

## What about hardware wallets?

Some users find hardware wallets an acceptable alternative to cold wallets as
they strike a balance between usability and security. [4] Yet at the time of
writing (early January 2018), there are no commercially available hardware
wallets for Monero, although Ledger support is reportedly [in the
works](https://www.reddit.com/r/Monero/comments/7de2pj/ledger_hardware_wallet_monero_integration_some/).

Note: **Malvarma is not a hardware wallet**, and that it will not store any
generated wallets.

## Development roadmap

11 January 2018: `0.1-alpha` relased for testing
End-January 2018: Release a version which provides 2-of-3 split keys using [Shamir's Secret Sharing](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing)

## Building Malvarma

### 1. Install dependencies:

For Ubuntu/Debian systems:

```bash
sudo apt install libguestfs-tools git python3 coreutils wget
```

### 2. Download and verify required files:

```bash
git clone git@github.com:weijiekoh/malvarma.git && \
cd malvarma && \
git clone git@github.com:weijiekoh/py2-monero-wallet-generator.git && \
sh download_raspbian.sh && \
```

If the `download_raspbian.sh` command fails, **do not continue** as the
Raspbian image may have been tampered with, and should be considered
unsafe.

### 3. Build a Malvarma image

```bash
python3 build.py
```

Note: `build.py` may require root permissions through `sudo` because
`guestmount` may not work for non-root users. If you encounter the following error:

```
libguestfs: error: /usr/bin/supermin exited with error status 1.
```
 
please run `sudo python3 build.py` instead.

#### What `build.py` does

`build.py` configures the Raspberry Pi image as follows:

- Disables WiFi and Bluetooth (in case it's run on a wireless-enabled Raspberry
  Pi)
- Removes some unnecessary system services (this makes it boot faster)
- Configures /etc/rc.local to perform these tasks upon boot:
    - Install `rng-tools` and check for sufficiently high entropy using `rngtest`
    - If `rngtest` passes with a maximum of 5 out of 1000 FIPS-140-2 failures, run
      `/python py2-monero-wallet-generator/gen_wallet.py` to generate and
      display the keys to a cold wallet 

Note that `build.py` is **not deterministic**. The output file
(`raspberry/malvarma-<version>.img`) is likely to differ over multiple runs,
even with the same Raspbian image and Malvarma code. This is probably because
`libguestfs` is also non-deterministic.  If anyone has suggestions on how to
deterministically modify raw disk images, please contact the author. In the
meantime, please audit the code yourself if you want to fully trust it.

### 4. Output `.img` file

The built `.img` file will be in the `build/` directory, and is ready to be
burned to a microSD card.

You may choose to emulate the image using QEMU, but since QEMU will modify the
image, **remember to delete and rebuild it when you are done**. 

```
sudo apt install qemu

sh run_qemu.sh && rm -rf build/malvarma-0.1-alpha.img
```

## What does *malvarma* mean?

Malvarma is Espernanto for cold.

## Want to buy me a coffee?

Please send XMR to
`4B4CBHf9qu8e67JWRYASFKTLT42fQCLHsDNDxzW71CjqRNtw8ZBN5rU1pPiVZEDPgv9rHfs23VpE9jpSrLpkWqmEQsbfj6i`
to support the author - thank you!

## Notes

[1] In Espernanto, *moneroj* is the plural of *monero*, or coin.

[2] A [cold boot attack](https://en.wikipedia.org/wiki/Cold_boot_attack), for
instance, may retrieve data computer memory even if it has been rebooted,
albeit within a timeframe of seconds to minutes.

[3] Some operating systems use on-disk swap files as temporary working memory
storage. Sensitive data, including private keys, [may find their way into swap
files](http://www.iusmentis.com/technology/encryption/crashcourse/security/),
and a determined adversary may be able to extract them.

[4] The [Ledger](https://www.ledgerwallet.com/products/ledger-nano-s), for instance,
uses a secure element to store private keys, which makes it easy to spend coins
without exposing any private keys.
