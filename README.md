WavPatcher 0.9.2b
-----------------

<p><img src="https://github.com/ckbaudio/wavpatcher/blob/main/img/wpscreenie.png" alt="wpscreenie.png" width="50%" height="50%" /><img src="https://github.com/ckbaudio/wavpatcher/blob/main/img/wpscreenie2.png" alt="wpscreenie2.png" width="50%" height="50%" /></p>

The purpose of WavPatcher is to replace a WAV\_EXTENSIBLE header subchunk sometimes found in WAV files that breaks compatibility on some equipment. This tool does not re-encode audio files but overwrites a 2-byte integer found in the header to invoke standard PCM. As such, this tool will not make non-standard multi-channel files (with more than 2 channels) compatible if your audio system does not already support it. 

### Downloads
The latest binaries can be found **[here](https://github.com/ckbaudio/wavpatcher/releases)**.

-----------------

> #### _What is WAV\_EXTENSIBLE?_
> 

[Wave extensible format](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-waveformatextensible) is an extension to the RIFF/WAV header which allows for custom or pre-defined channel-to-speaker mappings among other things. Vendors can create a unique descriptor table which contains information about the data following it so the audio system knows what to expect and how to appropriately decode with the correct audio precision and channel masks. Traditionally, it is common to define this data in simple 2-byte integers denoting type of data (PCM), sample rate (44.1kHz) and bit-depth (16), for example. WAV\_EXTENSIBLE uses more expandable methods for representing these attributes via GUID's that can handle higher sample rates and any number of pre-mapped channels which could be particularly useful for custom multi-channel configurations.

### The Problem

While most audio systems have maintained wav parsers, some systems will still not parse an WAV\_EXT implementation if the systems capabilities will never support multi-channel output or extremely high bit rates. However, some services and software write WAV\_EXT to generate a WAV file from raw PCM or rendered audio if the file uses an above CD-quality standard. While this is technically best practice, it isn't totally necessary for stereo files in most cases as traditional fmt code can still work with standard stereo types at higher-than-CD-quality. As such, these generic WAV files that use the ext flag can become unsupported by standalone media equipment even if the quality attributes of the file are otherwise compatible on the audio system in question.

### Why Use WavPatcher?

There isn't any easy or automated way to know whether wav_ext flags are present without a verbose metadata reader, if it even includes this info at all. Compounding this, large libraries of wav files make it laborsome to manually look for this problem. This tool doesn't do any form of encoding and only touches files that exhibit the EXT header flag. It also requires less horsepower and is faster than batch transcoding. This is particularly useful for DJ's as patching libraries shouldn't break files and nor mean having to re-import/re-analyse problematic files manually.

### What WavPatcher Can't Do

As mentioned above, WavPatcher is not an encoder/converter. If your audio system does not support playback of files beyond 48kHz/24-bit, this tool will not magically make them supported. For this, you must use some form of transcoding to convert your files to the specifications of your system.

As of now, WavPatcher is destructive, meaning that there is no 'undo' function and files are written directly on-disk. There may be some development going forward to implement an undo function. For now, use Read-only mode for checking libraries of data and always do a separate test folder to be sure media plays back on your system after patching.

_**Use this tool at your own risk.**_ 

### Building the Code (Optional)

I've included setup configuration files for both Windows and Mac. [Python3](https://www.python.org/downloads/) PIL ([Pillow](https://pillow.readthedocs.io/en/stable/installation.html)) pyinstaller and py2app are used for building just in case your system doesn't already have them installed.

For building on **Windows** systems, you can run from the main directory: `build_win.cmd`

For **MacOS**, open Terminal and CD into the wavpatcher-main directory and run: `./build_macos.sh` - You may need to run `chmod +x ./build_macos.sh` before if permissions are not automatically granted.

### Support
WavPatcher is written in Python 3, and builds are supported on MacOS and Windows.

#### For Windows
Since the binaries for Windows use pyinstaller, it's common for Windows Defender and AVG to generate a false-positive. It may be necessary to [add an exception to Windows Defender](https://support.microsoft.com/en-us/windows/add-an-exclusion-to-windows-security-811816c0-4dfd-af4a-47e4-c301afe13b26).

> Tested on:  
Mac OSX Sierra 10.12.6, Big Sur 11.0.1  
Windows 10 1809

### Known Issues
+ GUI sometimes has unpredictable update behavior on some MacOS systems.

+ Checking/patching may not fully close the file on interrupts if a user prematurely exits the application during checking/reading. This behaviour is unconfirmed but it's best to complete checking process before closing anyway, especially on external drives.

### About

Programming is only a hobby, so if you see redundancies (you probably will) or ways to improve my code feel free to fork :^)
