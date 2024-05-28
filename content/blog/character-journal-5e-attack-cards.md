---
title: "D&D 5e Character Journal Project"
date: "2024-05-28"
subtitle: "Part 1 of making better character sheets"
---

# Why Revamp the Character Sheet Anyway?

Because the [default](https://media.wizards.com/2022/dnd/downloads/DnD_5E_CharacterSheet_FormFillable.pdf) 5e character sheet kind of sucks, in my opinion.
For new players, there is way too much noise - there are loads of things they won't ever use, and plenty of space devoted to details that players never consult, and so little to what is frequently checked.
For experienced players, there is still too much noise, and they have to create all sorts of hacks to write down everything going on.
Eventually you end up having a half-hearted character journal, inventory sprawled in, at best, a crummy [bullet journal](https://bulletjournal.com/blogs/faq).
Don't believe me? Ask around.

>But that isn't noise!

I understand, but bear with me: the worst part of the character sheet by far comes during combat.
It's the slowest, most tedious part of any game.
Players have to balance the wants of the player and meta-knowlege, with in-universe knowlege and abilities.
Players are expected to know all of their options, and if we can speed that up, combat will fly.
I don't like the idea of using digital systems, or even looking things up on a smartphone midway through a game, as it can be a big distraction point.
So I planned this as something you could use for players of any skill level, and they can customize it as they go along.

## Why Start Here?

I actually started my process by drawing up a character sheet in Inkscape by taking only what I needed.
Explaining the process from there might be extremely daunting - so I decided to take a step back and explain things *inside-out*, from smallest unit to largest, so you can see the design language that ties it all together, without necessarily seeing how I got here (trial and error really).
I hope that by explaining the simplest element (basic weapon attacks) I can build up to spell cards, companion character sheets, player companion sheets (and how we can account for different classes), and finally the journal system itself.

## The Golden Rule

_The whole thing is still very much a work in progress._
I doubt many strangers read this blog - so if you know me, get in touch!
And if you don't, please reach out by creating a GitHub issue on this repository or the other one, and let's get in touch!

## Example Card

Here is an example card, for an artificer character: can you guess what it is and how it works?
It's in black and white because 

1. I don't own a colour printer;
2. I find it fun to colour these in.

{{< grid >}}

<div style="grid-row:'1/3'; padding: 1%;">
	{{< figure src="/img/5e-character-journal/attacks/rifle.png" width="375px" title="Basic 5e Rifle" caption="Prints to 2.5in x 3.5in @ 150 dpi, TCG format." class=floatleft >}}
</div>

<div style="grid-row:'2/3'; padding: 1%;">

### Top Left

**_Icon and Title_**: this part of the card establishes that the card is called "Rifle", and is referring to a gun.

### Top Right

**_Range_**: this part of the card establishes if it's a ranged attack, or requires touch, or has an area.
If it's blank, it's because it affects yourself.

In this case, it has a range of 80ft, with disadvantage up to 320ft (in the most common format, 16 squares / 64 squares).

### Bottom Right

**_Handedness_**: this establishes how many hands it takes to wield this.
It's a two handed gun, so I would darken both hands.

### Bottom Left

**_Dice to roll_**: We can see that the damage die here is a d12, and the attack roll is a d20.

**_Pre-calculations_**: We can also see that the values are pre-calculated: the first box is blank as some characters can use a stat other than STR/DEX to attack (in my artificer's case, it would be INT).

</div>
{{< /grid >}}

All of this was built in Scribus, and can be found in its [repository](https://github.com/jmsleiman/5e-character-journal).
Feel free to pull it down and rework it, or use it as-is.
I will eventually get around to uploading PDFs and PNGs in the Files section.
