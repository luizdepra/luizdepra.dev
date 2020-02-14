---
title: "SN-50: My new audacious project"
date: 2020-02-10
slug: sn-50-my-new-audacious-project
tags: ["rust", "fatansy-console", "sn-50"]
categories: ["SN-50", "Rust"]
series: ["SN-50 DevLog"]
draft: true
---

Without knowing it, my first contact with a fantasy console was 2011 in a [Humble Bundle][humble-bundle] of the sandbox game [Voxatron][voxatron], made by Lexaloffle. Years later I found out that Voxatron has a brother, [PICO-8][pico8], and that both are fantasy consoles. At the time I already knew that this kind of project would be a good way to learn more and have fun doing something new. But it felt too big, complex and distant to achieve, so I've shelved the idea until this moment.

## What is a Fantasy Console?

But what are Fantasy Consoles? Also known as Fantasy Computers, they are a kind of virtual machine that recreates the experience of making and playing games in a system with limited resources. Normally they simulate an imaginary machine, but this is not a rule carved in stone. The kind of limitations may vary for each system, but you will normally find restrictions about screen resolution, code length, sprites quantity, map sizes, number of color and cartridge (ROM) total size. All these constraints are carefully selected to provide a fun and challenging environment and aldo to encourage the developer to make small but expressive game.

{{< figure src="/images/sn50/pico8.gif" alt="PICO-8 Demo from lexaloffle.com" caption="PICO-8 Demo from lexaloffle.com" >}}

If you ever played a small game on [itch.io][itchio] or one made in game jams, you might have came across a splash or load screen that looks like a booting process of a computer. This is a very common feature and it is a good way to know that the game was made with a fantasy console ane with which one.

{{< figure src="/images/sn50/zelda-tic80.gif" alt="A TIC-80 demake of The Legend of Zelda: Breath of the Wild by Novemberisms" caption="A TIC-80 demake of The Legend of Zelda: Breath of the Wild by Novemberisms" >}}

The fantasy consoles community is quite big, mainly around [PICO-8][pico8]. But there are other big players such as the already mentioned [Voxatron][voxatron], [TIC-80][tic80], [LIKO-12][liko12] and [Pixel Vision 8][pixelvision8].

## SN-50

> And here comes a new challenger! Well... not quite yet.

{{< figure src="/images/sn50/sn50-logo.png" alt="The SN-50 ugly logo made by me" caption="The SN-50 ugly logo made by me" >}}

SN-50 is my newest project. Its status has been upgraded from "idea on a paper" to "in development". It will be a fantasy console, of course, but more flexible about its constraints. Developers will be able to choose which limitations their game will have. Screen resolution? Palettes with 4, 8 or 16 colors? 32k, 64k or 128k for cartridge size? You decide. But these are just examples, ok? I can't share more about limitations values because, to be honest, nothing is final yet.

About its features, I want to offer an experience similar to other fantasy consoles. SN-50 should include editors for sprites, glyphs, maps, sound effects, musics and code. A system to share cartridges and standalone players may be available too.

I'll be using Rust in this endeavour, always trying to use pure Rust crates. No promises about which windowing, graphic and audio libraries I'll be using, and I accepting suggestion.

Interested? You can follow SN-50's development from its [repository][sn80-repo], and maybe you can contribute too. Why not? Have I said that the project is Open Source?

If your asking yourself "why SN-50?", the name cames from the chemical element Tin: symbol **Sn** and atomic number **50**. Nerdy, right?

## What is next?

So, what is next? Right now I'm designing the cartridge file and its read and write routines. This project is quite big and challenging, it will force me to learn stuffs from game development that I've never touched before. So, stay tunned, I'm planing to write about everything I do and learn in the SN-50's article series. See you soon.


[humble-bundle]: https://www.humblebundle.com/
[itchio]: https://itch.io/
[voxatron]: https://www.lexaloffle.com/voxatron.php
[pico8]: https://www.lexaloffle.com/pico-8.php
[tic80]: https://tic.computer/
[liko12]: https://liko-12.github.io/
[pixelvision8]: https://www.pixelvision8.com/
[sn80-repo]: https://github.com/TinTeam/SN-50
