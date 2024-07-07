```
  ____             _        _ _   _             _    _____               
 | __ ) _ __ _   _| |_ __ _| | | | | ___   ___ | | _| ____|_  _____  ___ 
 |  _ \| '__| | | | __/ _` | | |_| |/ _ \ / _ \| |/ /  _| \ \/ / _ \/ __|
 | |_) | |  | |_| | || (_| | |  _  | (_) | (_) |   <| |___ >  <  __/ (__ 
 |____/|_|   \__,_|\__\__,_|_|_| |_|\___/ \___/|_|\_\_____/_/\_\___|\___|
made with love by: github.com/pxcs - p3xsouger - contributor - Sulaiman                                                                       
```

## Main features:
 - many of @OneOfEleven mods, including AM fix
 - @fagci spectrum analyzer (**F+5** to turn on)
 - @RE3CON multiband full freq ranges 14 MHz - 1789 MHz
 - @pxcs make a multi attack ( arsenal-mode )

## Radio performance

Please note that the Quansheng `BrutalHookExec` are not professional quality transceivers, their
performance is strictly limited. The RX front end has no track-tuned band pass filtering
at all, and so are wide band/wide open to any and all signals over a large frequency range.<br />

## User customization

You can customize the firmware by enabling/disabling various compile options, this allows
us to remove certain firmware features in order to make room in the flash for others.
<br>
You'll find the `options` at the top of "Makefile" ('0' = disable, '1' = enable) ..

```
ENABLE_CLANG                  := 0     **experimental, builds with clang instead of gcc (LTO will be disabled if you enable this)
ENABLE_SWD                    := 0       only needed if using CPU's SWD port (debugging/programming)
ENABLE_OVERLAY                := 0       cpu FLASH stuff, not needed
ENABLE_LTO                    := 1     **experimental, reduces size of compiled firmware but might break EEPROM reads (OVERLAY will be disabled if you enable this)
ENABLE_UART                   := 1       without this you can't configure radio via PC !
ENABLE_AIRCOPY                := 0       easier to just enter frequency with butts
ENABLE_FMRADIO                := 1       WBFM VHF broadcast band receiver
ENABLE_NOAA                   := 0       NOAA Weather broadcast alerts (receiption only in North America: Alaska, Canada, U.S.,...) A 1050 Hz Tone call demute the speaker at the beginning of every transmission. The 10 memory channels are replaced with the first 10 PMR channels. Use Sidekey for programable second call tone or >80ms roger beep sound mod with a 1050 Hz tone.
ENABLE_VOICE                  := 0       voice menu dialogues
ENABLE_VOX                    := 1       Audio Micro-Voice controlled PTT
ENABLE_ALARM                  := 0       TX alarms // BROKEN CODE?!
ENABLE_1750HZ                 := 0       side key 1750Hz TX tone (older style repeater access) // BROKEN CODE?!
ENABLE_PWRON_PASSWORD         := 0       power-on password stuff
ENABLE_BIG_FREQ               := 1       big font frequencies (like original QS firmware)
ENABLE_SMALL_BOLD             := 1       bold channel name/no. (by name + freq channel display mode)
ENABLE_KEEP_MEM_NAME          := 1       maintain channel name when (re)saving memory channel
ENABLE_WIDE_RX                := 1       full 18MHz to 1300MHz RX (though front-end/PA not designed for full range)
ENABLE_TX_WHEN_AM             := 1       allow TX (always FM) when RX is set to AM
ENABLE_F_CAL_MENU             := 0       enable/disable the radios hidden frequency calibration menu
ENABLE_TX_UNLOCK              := 1       TX all Bands 14 MHz to 1789 MHz. Hidden Menu -> F-Lock -> Select UNLOCKED. 
(TX harmonic content will cause interference to other services!)
ENABLE_CTCSS_TAIL_PHASE_SHIFT := 1       standard CTCSS tail phase shift rather than QS's own 55Hz tone method
ENABLE_BOOT_BEEPS             := 0       gives user audio feedback on volume knob position at boot-up
ENABLE_SHOW_CHARGE_LEVEL      := 1       show the charge level when the radio is on charge
ENABLE_REVERSE_BAT_SYMBOL     := 0       mirror the battery symbol on the status bar (+ pole on the right when enabled)
ENABLE_CODE_SCAN_TIMEOUT      := 0       enable/disable 32-sec CTCSS/DCS scan timeout (press exit butt instead of time-out to end scan)
ENABLE_AM_FIX                 := 1       dynamic adjust the frontend gains in AM mode to helo prevent AM demodulator saturation, ignore the on-screen RSSI level (for now)
ENABLE_AM_FIX_SHOW_DATA       := 0       show debug data for the AM fix (still tweaking it)
ENABLE_SQUELCH_MORE_SENSITIVE := 1       make squelch levels a little bit more sensitive - I plan to let user adjust the values themselves
ENABLE_FASTER_CHANNEL_SCAN    := 1       increases the channel scan speed, but the squelch is also made more twitchy
ENABLE_RSSI_BAR               := 1       enable a dBm/Sn RSSI bar graph level inplace of the little antenna symbols
ENABLE_AUDIO_BAR              := 1       display an audo bar, VU meter level when TX'ing
ENABLE_COPY_CHAN_TO_VFO       := 1       copy current channel into the other VFO. Long press Menu key ('M')
#ENABLE_SINGLE_VFO_CHAN       := 1       not yet implemented - single VFO on display when possible
#ENABLE_BAND_SCOPE            := 1       not yet implemented - spectrum/pan-adapter
```

## Compiler

arm-none-eabi GCC version 10.3.1 is recommended, which is the current version on Ubuntu 22.04.03 LTS.
Other versions may generate a flash file that is too big.
You can get an appropriate version from: [@](https://developer.arm.com/downloads/-/gnu-rm)

clang may be used but isn't fully supported. Resulting binaries may also be bigger.
You can get it from: [@](https://releases.llvm.org/download.html)

## Building

If you have docker installed you can use `compile-with-docker.bat`, the output files are created in `compiled-firmware` folder. This method gives significantly smaller binaries, I've seen differences up to 1kb, so it can fit more functionalities this way. The challange can be (or not) installing the docker itself.

## To compile directly in windows:

[Download latest Source Code](https://github.com/RE3CON/uv-k5-firmware-custom/archive/refs/heads/main.zip)

1. Open windows command line and run:
    ```powershell
    winget install -e -h git.git Python.Python.3.8 GnuWin32.Make
    winget install -e -h Arm.GnuArmEmbeddedToolchain -v "1.1.1.1"
    ```

2. Close command line, open a new one and run:
    ```powershell
    pip install --user --upgrade pip
    pip install crcmod
    
    mkdir c:\projects & cd /D c:/projects
    git clone https://github.com/pxcs/BrutalHookExec.git
    ```
3. From now on you can build the firmware by going to `c:\projects\BrutalHookExec` and running `cd config` `cd src` `win_make.bat` or by running a command line:
    ```powershell
    cd /D c:\projects\BrutalHookExec
    cd config then cd src
    win_make.bat
    ```
4. To reset the repository and pull new changes run:
    ```powershell
    cd /D c:\projects\BrutalHookExec
    git reset --hard & git clean -fd & git pull
    ```

<br>
I've left some notes in the win_make.bat file to maybe help with stuff.

## Credits

Many thanks to various people on Telegram for putting up with me during this effort and helping:

* [pxcs](https://github.com/pxcs)
* [Egzumer](https://github.com/egzumer)
* [OneOfEleven](https://github.com/OneOfEleven)
* [DualTachyon](https://github.com/DualTachyon)
* [Mikhail](https://github.com/fagci)
* [Andrej](https://github.com/Tunas1337)
* [Manuel](https://github.com/manujedi)
* [Matoz](https://github.com/spm81)
* @wagner
* @Lohtse Shar
* @Davide
* @Ismo OH2FTG
* @d1ced95

> THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
