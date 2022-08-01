# downsample

## Overview

`downsample` is a trivial script written in POSIX sh that converts
24-bit FLAC files to 16-bit with proper sampling rate scaling in a
recursive manner.

## Proper sampling rate scaling?

Downsampling lossless files to a common multiple may minimise the chance
of introducing various artefacts during the conversion. For example, a
sampling rate of 88.2 kHz shall be converted to 44.1 kHz, *not* 48 kHz.

## Usage

```
downsample DIR [DIR2..]
```

## Examples

Let's assume that we have a directory structure akin to the one outlined
below.

<details>
  <summary><i>directory structure before downsampling</i></summary>
<pre><code>music/
└── experimental/
    ├── musique concrete/
    │   └── Brume/
    │       └── 2012 - Fractisum/
    │           ├── 01 - Fractisum (Part 1).flac
    │           ├── 02 - Fractisum (Part 2).flac
    │           ├── cover.jpg
    │           └── lineage.txt
    └── noise/
        └── Masonna/
            └── 1991 - Split w. Violent Onsen Geisha/
                ├── 01 - Violent Onsen Geisha - Ultra Psychotic Piss Drunk Bunny Girl!.flac
                ├── 02 - Violent Onsen Geisha - Cock Guillotine 1965.flac
                ├── <...>
                ├── 12 - Masonna - Key.asasggkjkieeee.flac
                ├── 13 - Masonna - Sax.asqqqrtefdhkgjriukhvjbmosfjiejp.flac
                ├── cover.jpg
                └── lineage.txt</code></pre>
</details>

If we were to downsample all those files, we would either use

```
downsample music
```

or even

```
downsample musique\ concrete noise
```

and either of the two commands would produce the desired output. Now,
let's look at the directory structure after the invocation of the
script.

<details>
  <summary><i>directory structure after downsampling</i></summary>
<pre><code>music/
└── experimental/
    ├── musique concrete/
    │   └── Brume/
    │       └── 2012 - Fractisum/
    │           ├── <strong>resampled-16-44/</strong>
    │           │   ├── 01 - Fractisum (Part 1).flac
    │           │   └── 02 - Fractisum (Part 2).flac
    │           ├── 01 - Fractisum (Part 1).flac
    │           ├── 02 - Fractisum (Part 2).flac
    │           ├── cover.jpg
    │           └── lineage.txt
    └── noise/
        └── Masonna/
            └── 1991 - Split w. Violent Onsen Geisha/
                ├── <strong>resampled-16-48/</strong>
                │   ├── 01 - Violent Onsen Geisha - Ultra Psychotic Piss Drunk Bunny Girl!.flac
                │   ├── 02 - Violent Onsen Geisha - Cock Guillotine 1965.flac
                │   ├── <...>
                │   ├── 12 - Masonna - Key.asasggkjkieeee.flac
                │   └── 13 - Masonna - Sax.asqqqrtefdhkgjriukhvjbmosfjiejp.flac
                ├── 01 - Violent Onsen Geisha - Ultra Psychotic Piss Drunk Bunny Girl!.flac
                ├── 02 - Violent Onsen Geisha - Cock Guillotine 1965.flac
                ├── <...>
                ├── 12 - Masonna - Key.asasggkjkieeee.flac
                ├── 13 - Masonna - Sax.asqqqrtefdhkgjriukhvjbmosfjiejp.flac
                ├── cover.jpg
                └── lineage.txt</code></pre>
</details>

## Dependencies

The sole dependency is the `sox` package that usually includes both the
`sox` and `soxi` utilities. On Debian-based distros, the package can be
installed by executing the following command:

```
sudo apt-get install sox
```

On OpenBSD, it's as simple as running

```
doas pkg_add sox
```
