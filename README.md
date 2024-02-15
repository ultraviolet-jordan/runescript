# runescript

#### 0.1.png
```js
[opnpcu,_clue_master]
// We'll pick a random no. between 0-15 which is how many possible challenges there are.
def_int $random = random(16);
def_obj $clue = last_useitem;
// Do we have an elite clue scroll in our invents?
if (oc_category($clue)=elite_skill)
{
    // Change the clue scroll into a challenge scroll.
    // Inv_changeslot only takes type namedobj, so this'll have to do for now as I know what the last_useslot was.
    inv_setslot(inv,last_useslot,trail_elite_skill_challenge,1);
    // Set the challenge scroll's objvar to the value of the $random number.
    inv_setvar(inv,last_useslot,elite_skill_challenge,$random);
    // Set their progress to 0 so we can use it again later.
    %elite_skill_challenge_progress = 0;
    ~chatnpc("<p,happy>Ah, just what I was looking for. I have a challenge for you. If you complete it, I'll give you something.");
    return;
}
// If we don't have an elite clue, do we at least have a challenge clue?
if (oc_category($clue)=elite_skill_challenge_scroll)
{
    // Check whether we've even completed the challenge yet.
    if (%elite_skill_challenge_progress <= 0)
    {
        ~chatnpc("<p,happy>Bring your challenge back to me when you've completed it.");
        return;
    }
    // Get a new clue scroll, telling the proc to delete the last scroll we had, which for us, here, is the challenge scroll.
    ~get_new_elite_clue($clue);
    ~chatnpc("<p,happy>Fantastic! Here, have this.");
    return;
}
~chatnpc("<p,quiz>That's nice, but... I don't really need one of those.");

[opheld1,trail_elite_skill_challenge]
// The proc takes input type 'string'. Our enum's output type is 'string' so let's pass in the value of the challenge scroll
// objvar and pull out the corresponding challenge.
~full_trail_readclue(enum(int,string,trail_elite_skill_challenges,inv_getvar(inv,last_slot,elite_skill_challenge)));

// Called in the various files that require you to perform a skill challenge.
// Checks what skill you're doing & if you have the right challenge scroll for the skill...
// ...before returning true or false depending on if you have the right stuff.
[proc,elite_skill_check](int $elite_skill_challenge)(boolean)
// Just make a local variable with the string as I'm lazy.
def_string $completed = "<col=FF0000>Skill challenge completed.</col>";
// We need to be on a members world...
if (map_members = true)
{
    // We need to check what type of challenge they're trying to complete and if it's the right one.
```

#### 0.2.png
```js
// Read the clue
[opheld1,trail_hard_riddle_exp1]
~full_trail_readclue("Gold I see, yet gold I require.<br>Give me 875<br>if death you desire.");

// Code for this clue
[proc,trail_hard_riddle_exp1]
// Delete the clue....
inv_del(inv,trail_hard_riddle_exp1, 1);
// ....and give the player a casket!
inv_add(inv,trail_hard_riddle_exp1_casket, 1);
~objbox(casket,"You've found a casket!");

[opheld1,trail_clue_hard_riddle002_casket]
%namedobj = trail_clue_hard_riddle002_casket;
%s1 = "You've found another clue!";
// Jump to solve the clue.
~trail_solvehardclue;

//----------

```

#### 0.3.jpg
```js
[opheld1,trail_elite_emote_exp12] ~full_trail_readclue("Panic at the area of flowers meet now.<br>Equip Blue D'hide vambs, a dragon spear and<br>a rune plateskirt.");
[opheld1,trail_elite_emote_exp13] ~full_trail_readclue("Shrug in the Shayzien command tent.<br>Equip a blue mystic robe bottom, a rune kiteshield and<br>any bob shirt.");
[opheld1,trail_elite_emote_exp14] ~full_trail_readclue("Bow on the ground floor of a Legend's guild.<br>Equip Legend's cape, a dragon battleaxe and<br>an amulet of glory.");
[opheld1,trail_elite_emote_exp15] ~full_trail_readclue("Laugh in front of the gem store in Ardougne market.<br>Equip a Castlewars bracelet, a dragonstone amulet and<br>a ring of forging.");
[opheld1,trail_elite_emote_exp16] ~full_trail_readclue("Headbang in the Fight areba pub.<br>Equip a pirate bandana, a dragonstone necklace and<br>and a magic longbow.");
```

#### 0.4.jpg
```js
[oploc1,_zeah_door_left_closed]
// scripts\areas\zeah\arceuus\script\arceuus_characters.rs2
if (~arceuus_door_forbid = true) return;
def_coord $opposite = ~movecoord_loc_return(0,1);
sound_area(loc_coord,1,loc_param(opensound),1,0);
loc_del(100);
loc_add(loc_coord,loc_param(next_loc_stage),calc((loc_angle + 1) % 4),1,100);
loc_findallzone($opposite);
while (loc_findnext = true)
{
    if (loc_coord = $opposite & loc_category = zeah_door_right_closed)
    {
        loc_del(100);
        loc_add($opposite,loc_param(next_loc_stage),calc((loc_angle + 1) % 4),1,100);
        return;
    }
}
```

#### 0.5.png
```js
if ($dropint < 74)
{
    obj_add(npc_coord,ranarr_seed,2,200);
}
```

#### 0.6.jpg
```js
// Sigil table
if (random(585) = 159)
{
    $random2 = random(7);
    if ($random2 < 1) ~lootdrop_forbroadcast(elysian_sigil,1,$droppos,^lootdrop_duration,false,"",$broadcastpos,13,0);
    else if ($random2 < 4) ~lootdrop_forbroadcast(spectral_sigil,1,$droppos,^lootdrop_duration,false,"",$broadcastpos,13,0);
    else ~lootdrop_forbroadcast(arcane_sigil,1,$droppos,^lootdrop_duration,false,"",$broadcastpos,13,0);
}
```

#### 0.7.png
```js
{
    %blast_furnace_runite_bars = add(%blast_furnace...
    %blast_furnace_runite_ore = sub(%blast_furnace...
    %blast_furnace_coal = sub(%blast_furnace_coal...
    ~givexp(smithing,
    %temp = add(%temp, multiply(%returnvalue,10));
}
~min(%blast_furnace_adamantite_ore, divide(%blast_furnace...
~min(%returnvalue, sub(^blast_furnace...
if (%returnvalue > 0)
{
    %blast_furnace_adamantite_bars = add(%blast_furnace...
    %blast_furnace_adamantite_ore = sub(%blast_furnace...
    %blast_furnace_coal = sub(%blast_furnace_coal...
    %temp = add(%temp,multiply(%returnvalue,10));

}
~min(%blast_furnace_mithril_ore, divide(%blast_furnace...
~min(%returnvalue, sub(^blast_furnace...
if (%returnvalue > 0)
{
    %blast_furnace_mithril_bars = add(%blast_furnace...
    %blast_furnace_mithril_ore = sub(%blast_furnace...
    %blast_furnace_coal = sub(%blast_furnace_coal...
    %temp = add(%temp,multiply(%returnvalue,10));
}
~min(%blast_furnace_iron_ore, %blast_furnace_coal);
~min(%returnvalue, sub(^blast_furnace...
if (%returnvalue > 0)
{
    %blast_furnace_steel_bars = add(%blast_furnace...
    %blast_furnace_iron_ore = sub(%blast_furnace...
    %blast_furnace_coal = sub(%blast_furnace_coal...

    %temp = add(%temp,multiply(%returnvalue,10));
}
```

#### 0.9.png
```js
%temp=add(1,sub(random(153,%done));
```

#### 0.13.jpg
note: i believe this was some joke code for a streamer
```js
if (~pet_offer(3000,molepet) = true & name ! storgie) ~lootdrop_forbroadcast(molepet,1,null,false,$broadcastpos,$broadcastrange,0);
```

#### 0.14.jpg
```js
~if_openoverlay_hud(cata_boss);
runclientscript("cata_altar_update($a1,$a2,$a3,$a4)");
```

#### 0.15.jpg
```js
// Catacombs shard
~cata_ancient_shard_drop;
~trail_hardcluedrop(64,npc_coord);
```

#### 0.16.jpg
```js
[proc,olm_crystal_bomb]
```

#### 0.17.jpg
```js
^seaweed_rate=7500;
```

#### 0.71.png
```js
[ai_queue3,superior_pyrelord]
~npc_death(npc_coord);
~superior_slayer_uniques;
@pyrelorddrops;
```

#### 0.72.png
```js
if ($random < 1)
{
    ~lootdrop(npc_type,npc_coord,coins,20,^lootdrop_duration);
    return();
}
```

#### 100.0.png
```js
[label,gargboss_dusk_slash](seq $attack_anim, int $delay)
npc_anim($attack_anim,0);
anim(%com_defendanim,calc($delay * 30));
sound_synth(npc_param(attack_sound),1,0);
~npc_meleeattack;
def_int $damage = 0;
if (randominc(%combat_stat) > randominc(%com_slashdef))
{
    ~npc_meleedamage;
    $damage = randominc(%combat_stat);
    if (%prayer_protectfrommelee = ^true) $damage = scale(20,100,$damage);
    ~playerhit_n_melee(false,$damage,$delay);
}
else ~playerhit_n_melee(false,0,$delay);
```
