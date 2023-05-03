
# Toaplan Version 1  FPGA Implementation

FPGA compatible core of Toaplan Version 1 arcade hardware for [**MiSTerFPGA**](https://github.com/MiSTer-devel/Main_MiSTer/wiki) written by [**Darren Olafson**](https://twitter.com/Darren__O). FPGA implementation is based on schematics and verified against an Out Zone (TP-015 Conversion / TP-018), Vimana (TP-019), Tatsujin (TP-013B), and Rally Bike (TP-012).

The intent is for this core to be a 1:1 implementation of Toaplan V1 hardware. Currently in beta state, this core is in active development with assistance from [**atrac17**](https://github.com/atrac17) and [**𝕓𝕝𝕒𝕔𝕜𝕨𝕚𝕟𝕖**](https://github.com/blackwine).

![Toaplan_logo_shadow_small](https://user-images.githubusercontent.com/32810066/151543842-5f7380a4-9b29-472d-bc03-8cc04a579cf2.png)

## Supported Games

| Title                                                                   | PCB<br>Number | Status      | Released | ROM Set     |
|-------------------------------------------------------------------------|---------------|-------------|----------|-------------|
| [**Dash Yarou**](https://en.wikipedia.org/wiki/Rally_Bike)              | TP-012        | **W.I.P**   | No       | .250 merged |
| [**Tatsujin**](https://en.wikipedia.org/wiki/Truxton_%28video_game%29)  | TP-013B       | Implemented | **Yes**  | .250 merged |
| [**Hellfire**](https://en.wikipedia.org/wiki/Hellfire_%28video_game%29) | B90           | Implemented | **Yes**  | .250 merged |
| [**Zero Wing**](https://en.wikipedia.org/wiki/Zero_Wing)                | TP-015        | Implemented | **Yes**  | .250 merged |
| [**Horror Story**](https://en.wikipedia.org/wiki/Demon%27s_World)       | TP-016        | **W.I.P**   | No       | .250 merged |
| [**Same! Same! Same!**](https://en.wikipedia.org/wiki/Fire_Shark)       | TP-017        | **W.I.P**   | No       | N/A         |
| [**Out Zone**](https://en.wikipedia.org/wiki/Out_Zone)                  | TP-018        | Implemented | **Yes**  | .250 merged |
| [**Vimana**](https://en.wikipedia.org/wiki/Vimana_%28video_game%29)     | TP-019        | **W.I.P**   | No       | N/A         |

## External Modules

| Name| Purpose | Author |
|-----|---------|--------|
| [**fx68k**](https://github.com/ijor/fx68k)                     | [**Motorola 68000 CPU**](https://en.wikipedia.org/wiki/Motorola_68000)                   | Jorge Cwik       |
| [**t80**](https://opencores.org/projects/t80)                  | [**Zilog Z80 CPU**](https://en.wikipedia.org/wiki/Zilog_Z80)                             | Daniel Wallner   |
| [**y80e**](https://opencores.org/projects/y80e)                | [**Zilog Z180 CPU**](https://en.wikipedia.org/wiki/Zilog_Z180)                           | Sergey Belyashov |
| [**jtopl2**](https://github.com/jotego/jtopl)                  | [**Yamaha OPL2**](https://en.wikipedia.org/wiki/Yamaha_OPL#OPL2)                         | Jose Tejada      |
| [**tms320c1x**](https://github.com/srg320/TMS320C1X)           | [**Texas_Instruments TMS32010**](https://en.wikipedia.org/wiki/Texas_Instruments_TMS320) | Srg320           |
| [**yc_out**](https://github.com/MikeS11/MiSTerFPGA_YC_Encoder) | [**Y/C Video Module**](https://en.wikipedia.org/wiki/S-Video)                            | Mike Simone      |

# Known Issues / Tasks

- Sprite handler for Dash Yarou [Task]  
- HD647180X (Z180) implementation [Task]  
- TMS32010 DSP implementation [Task]  <br><br>
- Check sprite flicker on stage 5 (Out Zone)  [Issue]  

# PCB Check List

### Clock Information

| H-Sync       | V-Sync      | Source    | PCB<br>Number                                                |
|--------------|-------------|-----------|--------------------------------------------------------------|
| 15.556938kHz | 55.161153Hz | DSLogic + | TP-012<br><br>TP-016<br><br>TP-018                           |
| 15.556938kHz | 57.612182Hz | DSLogic + | TP-013B<br><br>B90<br><br>TP-015<br><br>TP-017<br><br>TP-019 |

### Crystal Oscillators

| Location | PCB<br>Number | Freq (MHz) | Use                                                                                             |
|----------|---------------|------------|-------------------------------------------------------------------------------------------------|
| X1       | Varies        | 10.00      | M68000 CLK (10 MHz)<br><br>HD647180x (10 MHz)                                                   |
| R1       | Varies        | 28.000     | Z80 CLK (3.5 MHz)<br><br>YM3812 CLK (3.5 MHz)<br><br>Pixel CLK (7 MHz)<br><br> DSP CLK (14 MHz) |

**Pixel clock:** 7.00 MHz

**Estimated geometry:**

_(Dash Yarou, Horror Story, Out Zone)_

    450 pixels/line  
  
    282 lines/frame  

_(Tatsujin, Hellfire, Zero Wing, Same!(3x), Vimana )_

    450 pixels/line  
  
    270 lines/frame  

### Main Components

Location | PCB<br>Number                                                | Chip                                                                   | Use                      |
---------|--------------------------------------------------------------|------------------------------------------------------------------------|--------------------------|
K 10-11  | All                                                          | [**Motorola 68000 CPU**](https://en.wikipedia.org/wiki/Motorola_68000) | Main CPU                 |
N 7-8    | TP-012<br><br>TP-013B<br><br>B90<br><br>TP-015<br><br>TP-016 | [**Zilog Z80 CPU**](https://en.wikipedia.org/wiki/Zilog_Z80)           | Sound CPU                |
M 1-2    | All                                                          | [**Yamaha YM3812**](https://en.wikipedia.org/wiki/Yamaha_OPL#OPL2)     | OPL2                     |
N/A      | TP-016                                                       | [**TMS32010**](https://en.wikipedia.org/wiki/Texas_Instruments_TMS320) | DSP MCU                  |
N/A      | TP-017<br><br>TP-019                                         | [**HD647180X**](https://en.wikipedia.org/wiki/Zilog_Z180)              | Sound CPU & I/O Handling |

### Custom Components

Location | Chip                   | Use                |
---------|------------------------|--------------------|
A 3-6    | **FCU-2**              | Custom Graphics IC |
H 9      | **NEC D65081R077**     | Custom Graphics IC |
E 1      | **TOAPLAN-02 M70H005** | Custom Toaplan IC  | <br>

# Core Features

### Native Y/C Output

- Native Y/C ouput is possible with the [**analog I/O rev 6.1 pcb**](https://github.com/MiSTer-devel/Main_MiSTer/wiki/IO-Board). Using the following cables, [**HD-15 to BNC cable**](https://www.amazon.com/StarTech-com-Coax-RGBHV-Monitor-Cable/dp/B0033AF5Y0/) will transmit Y/C over the green and red lines. Choose an appropriate adapter to feed [**Y/C (S-Video)**](https://www.amazon.com/MEIRIYFA-Splitter-Extension-Monitors-Transmission/dp/B09N19XZJQ) to your display.

### H/V Adjustments

- There are two H/V toggles, H/V-sync positioning adjust and H/V-sync width adjust. Positioning will move the display for centering on CRT display. The sync width adjust can be used to for sync issues (rolling) without modifying the video timings.

### Scandoubler Options

- Additional toggle to enable the scandoubler without changing ini settings and new scanline option for 100% is available, this draws a black line every other frame. Below is an example.

<table><tr><th>Scandoubler Fx</th><th>Scanlines 25%</th><th>Scanlines 50%</th><th>Scanlines 75%</th><th>Scanlines 100%</th><tr><td><br> <p align="center"><img width="" height="" src="FILLME"></td><td><br> <p align="center"><img width="" height="" src="FILLME"></td><td><br> <p align="center"><img width="" height="" src="FILLME"></td><td><br> <p align="center"><img width="" height="" src="FILLME"></td><td><br> <p align="center"><img width="" height="" src="FILLME"></td></tr></table> <br>

# PCB Information

<p align="center">Game Debugging
<table> <tr><th>Tatsujin</th><th>Debugging Features</th></tr><tr><td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="" height="" src="https://user-images.githubusercontent.com/32810066/171675808-91b88f14-3545-4c82-b239-0c93e06460df.png"></img></td><td><p align="left"> To access test mode, press P1 Start when the grid is displayed in "Service Mode".<br><br> To access sound test, press P2 Start when the grid is displayed in "Service Mode".<br><br> Turn the "Service Mode" dipswitch on in game for invulnerability.<br><br> Set the "Dip Switch Display" dipswitch to on in game to pause.<br><br> When the cabinet dipswitch is "Upright", you can use controls from both players.</p></td><tr><th>Hellfire</th><th>Debugging Features [hellfire/hellfire1]</th></tr><tr><td><img width="" height="" src="https://user-images.githubusercontent.com/32810066/171676182-50532067-4bb2-48b9-a671-a8b5aee69342.png"></img></td><td><p align="left"> In game, enable the "Invulnerability" dip and press P2 Start to pause; P1 Start <br>to resume.<br><br> When holding P1 Start and P2 Start, this will enable a slower framerate<br> in game.<br><br> When the cabinet dipswitch is "Upright", you can use controls from both players.</p></td><tr><th>Zero Wing</th><th>Debugging Features [zerowing]</th></tr><tr><td><img width="" height="" src="https://user-images.githubusercontent.com/32810066/171677271-c92a3171-2db7-461d-8279-158140cc14a9.png"></img></td><td><p align="left"> In game, enable the "Invulnerability" dip and press P2 Start to pause; P1 Start <br>to resume.<br><br> When holding P1 Start and P2 Start, this will enable a slower framerate<br> in game.<br><br> When the cabinet dipswitch is "Upright", you can use controls from both players.</p></td><tr><th>Out Zone</th><th>Debugging Features [outzoneb]</th></tr><tr><td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="" height="" src="https://user-images.githubusercontent.com/32810066/171676644-d4a0ef18-8854-4c22-be1c-16112fdc6eb9.png"></img></td><td><p align="left"> Set both "Debug" dipswitches to on and reset the game. Hold P2 Down during the <br>boot sequence. <br><br>This enables the CRTC registers to be programmed for a smaller VTOTAL enabling a <br>higher framerate by reducing the edges of the screen.<br><br> This changes the native refresh rate of Out Zone from 55.2Hz to 58.5Hz and the <br>resolution from 240p to 224p. It's fully functional in the core.<br><br> <p align="center">(**This is not correctly emulated in mame**)</p></td> </table> <br>

# Control Layout

| Game          | Joystick | Service Menu                                                                                                                         | Shared Controls | Dip Default |
| :---:         | :---:    | :---:                                                                                                                                | :---:           | :---:       |
| **Tatsujin**  | 8-Way    | <img width="" height="" src="https://user-images.githubusercontent.com/32810066/171675129-b1c64ea8-b345-4bc1-92f9-738a102eda67.png"> | No              | **Upright** |
| **Hellfire**  | 8-Way    | <img width="" height="" src="https://user-images.githubusercontent.com/32810066/171675135-4f852925-c3a8-4264-af9c-ac16417c0def.png"> | Co-Operative    | **Upright** |
| **Zero Wing** | 8-Way    | <img width="" height="" src="https://user-images.githubusercontent.com/32810066/171675142-75a94815-3bbb-4f60-a835-9a5bbb59a219.png"> | Co-Operative    | **Upright** |
| **Out Zone**  | 8-Way    | <img width="" height="" src="https://user-images.githubusercontent.com/32810066/171675149-e2f1a6fb-fe32-49aa-9880-6218eea2e34e.png"> | Co-Operative    | **Upright** |

<br>

- Upright cabinets by default use a **2L3B** control panel layout. Alternatively, they may share a <br>**1L3B** control panel layout and require players to switch their controller method.<br><br>
- If the cabinet type is set to table, the screen inverts for cocktail mode per player and has multiple controls.<br><br>
- Push button 3 may have no function in game, but corresponds to the original hardware and service menu.<br><br>

### Keyboard Handler

<br> - Keyboard inputs mapped to mame defaults for all functions. <br><br>

| Services                                                                                                                                                                                           | Coin/Start                                                                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <table> <tr><th>Functions</th><th>Keymap</th></tr><tr><td>Test</td><td>F2</td></tr><tr><td>Reset</td><td>F3</td></tr><tr><td>Service</td><td>9</td></tr><tr><td>Pause</td><td>P</td></tr> </table> | <table><tr><th>Functions</th><th>Keymap</th><tr><tr><td>P1 Start</td><td>1</td></tr><tr><td>P2 Start</td><td>2</td></tr><tr><td>P1 Coin</td><td>5</td></tr><tr><td>P2 Coin</td><td>6</td></tr> </table> |

| Player 1                                                                                                                                                                                                                                                                                                                                      | Player 2                                                                                                                                                                                                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <table> <tr><th>Functions</th><th>Keymap</th></tr><tr><td>P1 Up</td><td>Up</td></tr><tr><td>P1 Down</td><td>Down</td></tr><tr><td>P1 Left</td><td>Left</td></tr><tr><td>P1 Right</td><td>Right</td></tr><tr><td>P1 Bttn 1</td><td>L-CTRL</td></tr><tr><td>P1 Bttn 2</td><td>L-ALT</td></tr><tr><td>P1 Bttn 3</td><td>Space</td></tr> </table> | <table> <tr><th>Functions</th><th>Keymap</th></tr><tr><td>P2 Up</td><td>R</td></tr><tr><td>P2 Down</td><td>F</td></tr><tr><td>P2 Left</td><td>D</td></tr><tr><td>P2 Right</td><td>G</td></tr><tr><td>P2 Bttn 1</td><td>A</td></tr><tr><td>P2 Bttn 2</td><td>S</td></tr><tr><td>P2 Bttn 3</td><td>Q</td></tr> </table> |

# Acknowledgments

Many thanks to the following below who loaned hardware used during development:<br><br>

[**@owlnonymous**](https://twitter.com/owlnonymous) for loaning Out Zone (TP-015 Conversion)<br><br>
[**@cathoderaze**](https://twitter.com/cathoderaze)  for loaning Tatsujin (TP-013B)<br><br>
[**@90s_cyber_thriller**](https://www.instagram.com/90s_cyber_thriller/) for loaning Vimana (TP-019) and Outzone (TP-018)<br>

# Support

Please consider showing support for this and future projects via [**Darren's Ko-fi**](https://ko-fi.com/darreno) and [**atrac17's Patreon**](https://www.patreon.com/atrac17). While it isn't necessary, it's greatly appreciated.<br>

# Licensing

Contact the author for special licensing needs. Otherwise follow the GPLv2 license attached.
