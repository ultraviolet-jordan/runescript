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
