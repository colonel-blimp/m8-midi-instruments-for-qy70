# QY70 (XG level 1) MIDI instruments for Dirtywave M8

<!-- vim-markdown-toc GFM -->

* [Overview](#overview)
* [Contents](#contents)
* [Instruments](#instruments)
  * [Normal Voice Instruments](#normal-voice-instruments)
  * [SFX Voice Instruments](#sfx-voice-instruments)
  * [Drum Voice Instruments (XG drumkits)](#drum-voice-instruments-xg-drumkits)
  * [SFX Drum Voice Instruments (SFXkits)](#sfx-drum-voice-instruments-sfxkits)
  * [(CC-only) Mix-in utility instruments](#cc-only-mix-in-utility-instruments)
    * [Using mix-ins](#using-mix-ins)
    * [`SW+PEDAL CC.M8I`](#swpedal-ccm8i)
    * [`NRPN RPN CC.M8I`](#nrpn-rpn-ccm8i)
      * [NRPN table](#nrpn-table)
    * [`CHAN MODE CC.M8I`](#chan-mode-ccm8i)
* [Limitations / Workarounds](#limitations--workarounds)
  * [M8i only sends Bank Select LSB, not MSB](#m8i-only-sends-bank-select-lsb-not-msb)
  * [`chXX`: Multi-timbral instruments saftey workaround](#chxx-multi-timbral-instruments-saftey-workaround)
  * [M8 firmware 3.0.3a: Switching between certain instruments does not send `BANK`:`PG` messages](#m8-firmware-303a-switching-between-certain-instruments-does-not-send-bankpg-messages)
* [Changelog](#changelog)
  * [2.0.0](#200)
  * [1.0.0](#100)
* [Resources](#resources)

<!-- vim-markdown-toc -->

## Overview

This is a collection of `.m8i` MIDI instrument files to use with the [Yamaha
QY70].  It should also work reasonably well with the [Yamaha QY100] and
probably any synthesizer that implements [XG level 1].

It includes:

* All **538** instruments in the [QY70 List Book], pre-configured with common `CCx` FX
* **3** new M8-specific utility "mix-in" instruments with supplemental `CCx`―including RPNs & NRPNs
* A copy of each instrument for all 16 MIDI channels, as a multi-timbral safety feature


This is pack **<!-- PACK_VERSION -->`v2.0.0`<!-- END_PACK_VERSION -->**, tested
on M8 firmware **<!-- M8_FW_VERSION -->3.0.3<!-- END_M8_FW_VERSION -->**.

This instrument pack was heavily inspired by [pselodux]'s trailblazing pack of
all 470 "normal voice" QY70 instruments for the M8 v1 firmware (:trophy:).
Although this pack was constructed separately and differently, using pelodux's
original instruments informed so many of this pack's decisions it should be
considered v1.0.0.


## Contents

```
QY70/
├── ch01/
│   ├── BASS/
│   │   ├── AcidBass.m8i
│   │   …
│   │   └── X WireBa.m8i
│   ├── BRASS/
│   ├── +DRUMKITS/
│   ├── FX/
│   ├── GUITAR/
│   ├── ORCH/
│   ├── ORGAN/
│   ├── PERC/
│   ├── PERC-CHR/
│   ├── PIANO/
│   ├── PIPE/
│   ├── REED/
│   ├── +SFX/
│   ├── +SFXKITS/
│   ├── STRINGS/
│   ├── SYNTH-FX/
│   ├── SYNTH-LD/
│   ├── SYNTH-PAD/
│   ├── WORLD/
│   └── _UTIL/
├── ch02/
…
├── ch16/
└── README.md
```


At the top level, there are `chXX/` folders for MIDI channels 1–16.
Each contains its own copy of:

  * **473** `.m8i` files for the QY70's built-in **["Normal Voice" instruments]**
    * Arranged in subfolders based on the categories in the [QY70 List Book]
    * Send Bank LSB & Program Change messages on MIDI channel `chXX`
    * 10 standardized MIDI `CCx` fx
  * **45** `.m8i` files for the QY70's built-in **["SFX Voice" instruments]** (`+SFX/`)
  * **18** `.m8i` files for the QY70's built-in **["Drum Voice" Instruments (XG drumkits)]** (`+DRUMKITS/`)
  * **2** `.m8i` files for the QY70's built-in **[SFX Drum Voice Instruments (SFXkits)]** (`+SFXKITS/`)
  * **3** `.m8i` files for supplemental [(CC-only) **mix-in utility instruments**][mixins] (`_UTIL/`)
    * Send CCs on MIDI channel `chXX` , but _don't_ change the program or bank
    * Provide supplementary `CCx` fx, including:
      * Switches & pedals (including all three portamento parameters)
      * RPNs/NRPNs
      * MIDI channel mode messages (ex: "All Notes Off")


## Instruments

### Normal Voice Instruments

The full XG "Normal Voice" list is documented on pages 2–3 of the [QY70 List
Book].

Each normal voice instrument provides a standardized set of 10
"common" CCs:

| FX  | CC   | Purpose                | Notes                             |
| --- | ---- | ---------------------- | --------------------------------- |
| CCA | CC01 | Mod Wheel              |                                   |
| CCB | CC11 | Expression             |                                   |
| CCC | CC10 | Panpot                 |                                   |
| CCD | CC05 | Portamento time        |                                   |
| CCE | CC65 | Portamento on/off      | 0x00-0x3F: Off  0x40-0x7F: On     |
| CCF | CC91 | Reverb Send            |                                   |
| CCG | CC93 | Chorus Send            |                                   |
| CCH | CC94 | Variation Send         | Only when Connection = 1 (System) |
| CCI | CC74 | Filter Cutoff          | 0x00: -64 0x40: 0 0x7F: +63       |
| CCJ | CC71 | Filter Resonance       | 0x00: -64 0x40: 0 0x7F: +63       |


Note that the filter CCs (`CCI`) and (`CCJ`) are bipolar and default to center
at `0x40`.

By default, XG "Variation send" (`CCH`) effect is **delay**.  It can be set
to different effects (distortion, 3-band EQ, etc) directly on the device or
via Sysex. There is a full list of possible variation effects on page 9 of
the [QY70 List Book].


**XG patch name trivia**

* Instrument with names that begin with "`S.`" ( `S.SlwStr`), or begin or end
  with `St` (`StBrsSec`, `MidTGtSt`) are stereo.
* Instrument names that end with a capital "`K`" (`ElGrPnoK`) use "Key Scale
  Panning"―lower keys are panned left and higher keys are panned right.
* Instrument names followed by a plus (`+`) on the QY70 display are non-GM
  variations

Note that the M8 file browser displays instrument file names in upper case.
You can view the true mixed-case XG instrument names from the instrument
screen (after loading the `.m8i` file) or by viewing the `.m8i` files from an
external file browser that displays mixed-case names.


### SFX Voice Instruments

SFX voices work almost the same as normal voices, except they use a bank select
MSB of `0x40` "common" CCs.

The full XG "SFX Voice" list is documented on the right side of page 3 in the
[QY70 List Book].


| FX  | CC   | Purpose                | Notes                                  |
| --- | ---- | ---------------------- | -------------------------------------- |
| CCA | CC01 | Mod Wheel              |                                        |
| CCB | CC11 | Expression             |                                        |
| CCC | CC10 | Panpot                 |                                        |
| CCD | CC00 | Bank Select MSB        | Default value sends bank MSB each hit  |
| CCE | CC32 | Bank Select LSB        | Can be used to contradict `BANK` value |
| CCF | CC91 | Reverb Send            |                                        |
| CCG | CC93 | Chorus Send            |                                        |
| CCH | CC94 | Variation Send         | Only when Connection = 1 (System)      |
| CCI | CC74 | Filter Cutoff          | 0x00: -64 0x40: 0 0x7F: +63            |
| CCJ | CC71 | Filter Resonance       | 0x00: -64 0x40: 0 0x7F: +63            |


SFX voice instrument use the same CCs as normal voice instruments, except for
`CCD` (CC00/Bank Select MSB) and `CCE` (CC32/Bank Select LSB).

The default value `CCD` is required because the M8 MIDI instruments do not send
bank MSB.  As a side effect, the instrument's default bank MSB is sent before
each note.

To prepare a MIDI channel to switch back to a normal voice instrument while using
an SFX voice: use the fx `CCD80` prior to playing the normal voice instrument's
first note.


### Drum Voice Instruments (XG drumkits)


The full XG "Drum Voice" instrument list is documented on pages 4–8 of the [QY70 List
Book].

| FX  | CC   | Purpose                | Notes                                  |
| --- | ---- | ---------------------- | -------------------------------------- |
| CCA | CC98 | NRPN MSB               | NRPNs can be used to edit drum hits    |
| CCB | CC99 | NRPN LSB               | NRPNs can be used to edit drum hits    |
| CCC | CC06 | Data entry MSB         | NRPNs can be used to edit drum hits    |
| CCD | CC00 | Bank Select MSB        | Default value sends bank MSB each hit  |
| CCE | CC32 | Bank Select LSB        | Can be used to contradict `BANK` value |

* The default value `CCD` is required because the M8 MIDI instruments do not send bank MSB.
  * As a side effect, the instrument's default bank MSB is sent before each note.
* NRPN & Data entry CCs are provided because NRPNs can customize a number of
  parameters for individual drumkit notes

To prepare a MIDI channel to switch back to a normal voice instrument while using
an Drumkit voice: use the fx `CCD80` prior to playing the normal voice instrument's
first note.


### SFX Drum Voice Instruments (SFXkits)

SFXkits are collections of SFX voice instruments compiled into (very sparse)
drumkits.  They are documented on page 8 of the [QY70 List Book], at the end of
the "Drum Voice" instrument list.

| FX  | CC   | Purpose                | Notes                                  |
| --- | ---- | ---------------------- | -------------------------------------- |
| CCA | CC98 | NRPN MSB               | NRPNs can be used to edit drum hits    |
| CCB | CC99 | NRPN LSB               | NRPNs can be used to edit drum hits    |
| CCC | CC06 | Data entry MSB         | NRPNs can be used to edit drum hits    |
| CCD | CC00 | Bank Select MSB        | Default value sends bank MSB each hit  |
| CCE | CC32 | Bank Select LSB        | Can be used to contradict `BANK` value |

SFXkits use the same CC tables as the XG drumkits


### (CC-only) Mix-in utility instruments

The QY70 responds to many CC messages, but a single M8 MIDI instrument
supports a maximum 10 `CCx` fx.

As a workaround: each channel folder contains an additional `_UTIL/` directory
containing special "mix-in" utility instruments to provide supplementary CCs.
Unlike the normal voice instruments, mix-ins don't send bank or program
messages.

#### Using mix-ins

Mix-in instruments provide additional `CCx` fx and send to the same MIDI
channel as their folder's normal voice instruments.  Using multiple instruments
to send to the same MIDI channel can pose challenges on the M8―especially if
they are playing simultaneously.

Mix-in instruments are intended to be used _only_ for their extra `CCx` fx and
not MIDI notes.  But to use a mix-in's fx, it must first be triggered in the
`I` column of a phrase.  Unless you are careful, this could send unwanted MIDI note-on & note-off messages.

Some tips:

* Trigger mix-in instruments at `V00` to avoid sending unwanted MIDI note-ons.
  * **But be careful:** even at `V00`, triggering an instrument will kill any
    remaining events from the previous instrument.
    If that was a MIDI or external instrument, a MIDI note-off message
    will be sent to _that_ instrument's MIDI channel.
* Once triggered, you can keep using a mix-in instrument's `CCx` fx and table
  in that track until a different instrument is triggered.
  * If you can keep that track clear of other instruments, you can wait through
    many empty chains to send the mix-in's CCs without needing to trigger it
    again.
  * **But be careful:** M8 `CCx` fx are relative.  If precise absolute values
    are needed by the target CC (NRPNs, CC84), it will probably be
    necessary to re-trigger the instrument anyway to ensure each value always
    starts from `00`.
  * (Keep this in mind while placing `REP`s.)
* Always preview mix-in instruments using the same MIDI channel (`chXX/`) folder
  as the target voice instrument
* MIDI messages to a specific channel can come from any M8 track―simultaneously, if needed.
  * Keep mix-in instruments on different tracks from their MIDI channel's
    normal voice instruments while it's playing.
  * You can put mix-ins into any track(s) where they can fit and they will
    still affect the same voice on the QY.


Each mix-in utility instrument provides a related collection of CCs:

#### `SW+PEDAL CC.M8I`

The `_UTIL/SW+PEDAL CC.M8I` instrument provides all switch and pedal CCs, as
well as a few basic sound controls (including XG's enigmatic AC1).  Some
controls overlap with the voice instruments' CCs.

| FX  | CC   | Purpose            | Notes                         |
| --- | ---- | ------------------ | ----------------------------- |
| CCA | CC64 | Sustain on/off     | 0x00-0x3F: Off  0x40-0x7F: On |
| CCB | CC65 | Portamento on/off  | 0x00-0x3F: Off  0x40-0x7F: On |
| CCC | CC66 | Sostenuto on/off   | 0x00-0x3F: Off  0x40-0x7F: On |
| CCD | CC67 | Soft pedal on/off  | 0x00-0x3F: Off  0x40-0x7F: On |
| CCE | CC05 | Portamento time    |                               |
| CCF | CC84 | Portamento Control |                               |
| CCG | CC16 | AC1 controller     |                               |
| CCH | CC07 | Main volume        |                               |
| CCI | CC10 | Panpot             |                               |
| CCJ | CC11 | Expression         |                               |

A quick review of how the QY70 responds to the switches/pedals:

* **Sustain**: Sustains all notes playing while it's active

* **Sostenuto**: Sustains only notes that were _already playing_ when it was
  activated

* **Soft Pedal**: Lowers the volume of any notes played.  It should also
  "damper" the sound's character, but that only seems to affect certain Piano
  patches

* **Portamento**: When on, notes glide from pitch of the previous note.
  * Portamento time (`CCE`/CC05) controls the glide length
  * Portamento control (`CCF`/CC84) resets the previous note for the purposes of gliding to  the absolute number of the CC.
  * :warning: If you set CC84, the next note will glide from that value at the rate set by CC05―even if the portamento switch (`CCB`/CC65) isn't activated!

The **AC1 controller** (`CCG`) is a special XG-only effects control (the dots
on pages 9–11 of the [QY70 List Book] show what it can affect).  However: by
default AC1 is _completely useless_ on the QY70:

* Its initial modulation strength is 0 :faceplam:
* You can't change it without using Sysex :facepalm::facepalm:
* If you manage to turn it on: AC1 modulates a single parameter (usually
  Dry/Wet) on each reverb and most of the variation effects
  ...but _only_ when VARIATION is set to INSERT mode :facepalm::facepalm::facepalm:

The Sysex string to maximize AC1's modulation strength is `F0 43 10 4C 02 01 5F
7F F7`.  You can't send this from the M8, but you can punch it directly into
the QY70's song/pattern event list by selecting the event **`XG Exc Effect >
AC1Var`**  and setting it to either extreme (+63 or -64) and playing the
song/pattern to execute the event.



#### `NRPN RPN CC.M8I`

The `_UTIL/NRPN RPN CC.M8I` mix-in instrument provides CCs for  NRPNs & RPNs,
and a table with common NRPNs to trigger using `TH0`.

NRPNs are the _only_ way you can use CCs to affect XG vibrato parameters or the
complete set of EG parameters (for some reason there is no single CC for EG
decay).  They can also customize individual drumkit notes!

NRPNs and RPNs use at least three CCs to change, which must be sent in order:
* N/RPN MSB
* N/RPN LSB
* Data entry MSB
* (RPNs only) Data entry LSB

**Warning:** M8 `CCx` fx values are relative, but RPN & NRPN
triggers are absolute. To make sure you select the correct N/RPN MSB/LSB
values, always _trigger_ this mix-in instrument on the same step.
This will ensure that all `CCx` fx start from 0 and send the exact value
you specify on the step.

(As a reminder: to _trigger_ an M8 instrument, put the instrument
number in the phrase's `I` column on that step.  This will
interrupt any previously playing instrument and tables on that track.)


| FX  | CC    | Purpose        | Notes                |
| --- | ----  | -------------  | -------------------- |
| CCA | CC98  | NRPN MSB       |                      |
| CCB | CC99  | NRPN LSB       |                      |
| CCC | CC06  | Data entry MSB |                      |
| CCD | CC100 | RPN MSB        |                      |
| CCE | CC101 | RPN LSB        |                      |
| CCF | CC038 | Data entry LSB |                      |
| CCG | CC96  | Data entry inc | 0x7F: increment RPN  |
| CCH | CC97  | Data entry dec | 0x7F: decrement RPN  |
| CCI |       |                |                      |
| CCJ |       |                |                      |

Pages 45―46 of the [QY70 List Book] cover the available RPNS & NRPNs

##### NRPN table

To simplify sending N/RPNs from the M8, the mixin's table is pre-populated with
MSB+LSB triggers for common NRPNs.

**To use:** Select an NRPN with `THO` and set the data entry MSB value with
`CCC` (CC05). All further `CCC` values will continue to modify that NRPN until
another NRPN is triggered or the special NRPN reset message (`THO0E`) is sent.

**Example:**

  * To set Vibrato Rate to 128 (maximum):

    ```
    THO01 CCC7F
    ```

| Row | Purpose         | Notes                             |
| --- | --------------- | --------------------------------- |
| 01  | Vibrato Rate    | CCC = 0x00: -64 0x40: 0 0x7F: +63 |
| 03  | Vibrato Depth   | CCC = 0x00: -64 0x40: 0 0x7F: +63 |
| 05  | Vibrato Delay   | CCC = 0x00: -64 0x40: 0 0x7F: +63 |
| 08  | EG Attack time  | CCC = 0x00: -64 0x40: 0 0x7F: +63 |
| 0A  | EG Decay time   | CCC = 0x00: -64 0x40: 0 0x7F: +63 |
| 0C  | EG Release time | CCC = 0x00: -64 0x40: 0 0x7F: +63 |
| 0E  | NRPN Reset      | No data byte; just use `THO0E`    |


**A note about XG LFO (vibrato)**

The "Vibrato" parameters actually control the instrument's single LFO. In XG
MIDI, this LFO can also be configured to affect amplitude, pitch, and/or
filter, controlled by the mod wheel, pitch bend, and/or channel aftertouch.

Unfortunately: these extra mappings are impossible to configure without using
Sysex (which the M8 doesn't send).  As a result, the LFO is stuck modulating
whatever the voice is configured to use it for. Practically every instrument on
the QY just uses the instrument LFO to affect pitch, controlled by the mod
wheel.  And so: it's "vibrato."


#### `CHAN MODE CC.M8I`

The `_UTIL/CHAN MODE CC.M8I` instrument has CCs to send MIDI channel mode
messages. All of them are triggered with the value `00`.


| FX  | CC    | Purpose               | Notes                                 |
| --- | ----  | -------------         | ------------------------------------- |
| CCA | CC120 | All sound off         | 0x00: trigger                         |
| CCB | CC121 | Reset all controllers | 0x00: trigger                         |
| CCC | CC123 | All notes off         | 0x00: trigger                         |
| CCD | CC124 | Omni mode off         | 0x00: trigger                         |
| CCE | CC125 | Omni mode on          | 0x00: trigger                         |
| CCF | CC126 | Mono (ch mode 2)      | 0x00: trigger                         |
| CCG | CC127 | Poly (ch mode 3)      | 0x00: trigger                         |
| CCH |       |                       |                                       |
| CCI |       |                       |                                       |
| CCJ | CC0   | Bank Select MSB       | Use `CCJ00` to reset bank to normal   |

`CCA00` (All sound off) is particularly useful if you need to quickly silence
hanging instruments from the M8.

See page 44–45 of the [QY70 List Book] for descriptions of each channel mode
message



## Limitations / Workarounds

### M8i only sends Bank Select LSB, not MSB

M8 MIDI instruments' `BANK` setting only handles the MIDI Bank Select LSB
(CC32)―it does not affect Bank Select MSB (CC0) at all.  XG MIDI uses the Bank
Select MSB to partition instruments into several categories:

| Bank Select MSB | XG Voice category          | Notes  |
| --------------- | -------------------------- | ------ |
| 0x00            | Normal Voice               |        |
| 0x40            | SFX voice                  |        |
| 0x7E            | SFX kit                    |        |
| 0x7F            | Drum voice / Rhythm kit    |        |

Bank Select MSB is 0 unless changed, so if you are only using normal voice
instruments, you will never need to send it.

The `.m8i` instruments for the other instrument categories (SFX voice, Drum
Voice, SFX kit) use `CCD` to Bank Select MSB (CC0), with the default set to
their bank MSB.  This ensures that the bank select message are always sent in
the correct order, but at the expense of sending C00 before _every note_.

For this reason, the "Normal voice" instruments do _not_ include Bank Select
MSB (CC0).  This prevents unecessary CC0 traffic and keeps all 10 `CCx` slots
for common CCs―but the trade-off is that :warning: playing a normal voice
instrument after another kind of voice **does not automatically send the bank
select MSB back to normal.** :warning:

**Workaround:** When you are using a SFX/drumkit/SFXkit instrument, send `CCD80`
just before switching to a normal voice instrument.  This sends the correct
Bank Select MSB ahead of the normal instrument sending its natice Bank Select LSB
& Program Change.



### `chXX`: Multi-timbral instruments saftey workaround

The multiple `chXX/` folders are a multi-timbral safety feature: the M8
instrument browser previews MIDI instruments by sending their program + bank
changes to the channel _saved in each `.m8i` file_.  This can make it
dangerous to preview instruments for a multi-timbral device like the QY70
unless the previewed `.m8i` files are already configured for the target MIDI
channel.

:warning: Browsing MIDI instruments on the wrong MIDI channels **risks
accidentally reconfiguring unrelated voices.**

This is especially risky when mixing "normal voice" instruments with
instruments that send CC0/Bank Select MSB messages, because it won't be
possible to to switch back to the normal voice instrument without explicitly
sending Bank Select MSB 0.


### M8 firmware 3.0.3a: Switching between certain instruments does not send `BANK`:`PG` messages

While testing this pack on M8 FW 3.0.3a, it was observed that changing between certain
instrument pairs (in either direction) from the M8 does not send the new
instrument's BANK LSB (CC32) + program change messages.  This is very rare, but
repeatable between affected instrument pairs.

Some example pairings (in `BANK:PG` format) include:

* `66:101` <-> `67:101`
* `64:101` <-> `65:101`
* `000:102` <-> `008:102`

**Workaround:** Change to an unaffected instrument to force the change messages,
then change to the target instrument.

It is very likely this issue will be fixed in future FW updates.

## Changelog

### 2.0.0
2023-04-25 ― ineffable-twaddle
- Update instruments to M8 v3 instrument format
- Move Bank LSB CC into the instrument settings to free up `CCA`
- Add common QY70 CCs to each voice instrument
- Add new mix-in utility instruments to provide additional CCs
- Add table to trigger common NRPNs
- Make 16 channel variations of all instruments to prevent
  multi-timbral preview disasters
- Add instruments that require non-zero bank MSB CCs:
  - SFX
  - Drumkits
  - SFX kits
- Add documentation

### 1.0.0
2021-01-24 ― pselodux
- [Original 473 QY70 defs][1] in M8 v1 instrument format :trophy:

## Resources

* [QY70 List Book]
* [QY70 Owner's Manual]
* [The Yamaha QY70 and QY100 User Group](https://groups.io/g/YamahaQY70AndQY100)
* [Dirtywave resources page]
* [Dirtywave discord]

["Normal Voice" instruments]: #normal-voice-instruments
["Drum Voice" Instruments (XG drumkits)]: #drum-voice-instruments-xg-drumkits
["SFX Voice" Instruments]: #sfx-voice-instruments
[SFX Drum Voice Instruments (SFXkits)]: #sfx-drum-voice-instruments-sfxkits

[mixins]: #cc-only-mix-in-utility-instruments
[Dirtywave discord]: https://discord.gg/WEavjFNYHh
[Dirtywave M8 Tracker]: https://dirtywave.com/products/m8-tracker
[Dirtywave resources page]: https://dirtywave.com/pages/resources-downloads
[Yamaha QY70]: https://www.yamaha.com/en/about/innovation/collection/detail/2080/
[Yamaha QY100]: https://www.yamaha.com/en/about/innovation/collection/detail/2091/
[QY70 List Book]: https://usa.yamaha.com/files/download/other_assets/6/317936/QY70E2.PDF
[QY70 Owner's Manual]: https://usa.yamaha.com/files/download/other_assets/8/318058/QY70E1.PDF
[XG level 1]: http://www.thewhippinpost.co.uk/midi/xg/xg-midi.htm
[1]: https://discord.com/channels/709264126240620591/754405144325521530/803117850940145674
[pselodux]: https://github.com/pselodux
