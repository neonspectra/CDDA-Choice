# CDDA Community Tweaks

> *If you need to force people to play with your mechanic to get feedback[,] you probably need to go back to the drawing board on the mechanic.*
> --[u/Hilde_Garde_Party](https://www.reddit.com/r/cataclysmdda/comments/12e2748/development_strategy/jf9n5c4/)

> *Wanting a toggle is the only feedback this situation needed.*
> --[u/Raven776](https://www.reddit.com/r/cataclysmdda/comments/12e2748/development_strategy/jfaysyy/)

> *One thing devs might be missing is the fact that what made this game fun for a lot of people is how the experience was for the user. We could personalize our experience to our hearts content.*
> --[u/Kurt_Wolfgang](https://www.reddit.com/r/cataclysmdda/comments/12e2748/development_strategy/jfabz17/)

CataclysmDDA is a singleplayer sandbox game that is renowned for its rich modding systems and customisation options that encourage players to extract fun from the game however they choose.

The purpose for this guide is to be a repository for commonly requested community tweaks that the developers have explicitly chosen not to provide toggle options for. Pull requests and additional tweak guides are welcome.

## Disable Portal Storms
*[Fix Source](https://www.reddit.com/r/cataclysmdda/comments/uics3f/comment/i7c8c7g)*

> *I guess the difficulty with this idea is just that portal storms are controversial; some people are happy for a reason to need to fortify bases and that's fine.*
> *However, portal storms kill types of easier agrarian play that were viable before (particularly if they go back to slaughtering your livestock,) and fans of the game that like those playstyles aren't going to like that, period.*
> *I don't think it's possible to make a compromise version of the event that would satisfy both the "I want to do a fortress defense" people and the "I want to live in a lean-to all summer" people, so "just play it and give feedback" won't fix the problem.*
> --[u/Martian_Astronomer](https://www.reddit.com/r/cataclysmdda/comments/12e2748/development_strategy/jf9ecjb/)

### Applying the Fix
You can delete the EOC that causes them to run:

1. Head to `data/json/portal_storm_effect_on_condition.json` and open it with your text editor of choice;
2. Look for the json definition with the ID `EOC_PORTAL_STORM_WARN_OR_CAUSE_RECURRING;`
3. Delete the whole def, including the brackets around it.

This is the code that actually physically causes portal storms to run, and deleting it will stop them from happening while keeping the code in the game so nothing breaks. Scenarios with portal storms active (like `Splintering Reality`) will still have the portal storm at the start, but no other ones will happen. This will obviously cause some balance issues with the scenarios about more frequent or stronger portal storms since they just turn into free points, but you can just not use 'em.

## Disable Proficiencies
*[Fix Source](https://www.reddit.com/r/cataclysmdda/comments/l6ngr1/is_there_anyway_to_disable_proficiencies/)*

The inclusion of proficiencies adds drawn-out practice requirements to fully craft many recipes. The extended time and resource costs brought on by the proficiencies system may help round out the gameplay for a more comprehensive long-run experience, but it also negatively impacts the creativity that is possible in most challenge starts. As such, the inclusion of the proficiency system is best left optional.

### Option 1: Use the debug menu

Use the debug menu to learn all proficiencies.

Downsides for this option:
- It will clutter your @ menu.
- It will disable achievements (but you can easily re-enable them in the debug menu)

### Option 2: Remove proficiencies from all recipes

Remove the proficiencies from every recipe in the CDDA JSON. This is most easily done by iteratively running a `sed` command on all of CDDA's recipre json to remove the `proficiency` lines.

Steps:
1. Navigate to `data/json/recipes/` in your terminal
2. Run the following command in your terminal: `find . -name "*.json" | xargs -I{} sed -i 's/proficiencies/\/\/proficiencies/g' {}`

Downsides for this option:
- You will have to apply this tweak every time you update your game
- You need to have the GNU core utilities on your machine to run the above command. Windows users are out of luck unless you use [WSL](https://learn.microsoft.com/en-us/windows/wsl/install).

## Disable Skill Rust
*Fix Source: TBA*

> *They want player feedback and here’s my feedback.*
> *I extremely dislike skill rust. I dislike the concept, I dislike the implementation, and trying to account for and prevent it is unpleasant.*
> *I don’t have much time to play games, so I want to make progress in them when I do. The concept that by spending time in that game some of that progress is undone makes me not want to play it.*
> --[u/PsychoTexan](https://www.reddit.com/r/cataclysmdda/comments/12eiwpx/comment/jfm09qu/)

**TBA:** I play on Stable so I haven't figured out the fix for this change yet because the PR to remove this toggle was just recently merged. If you know the tweak, please feel free to PR this repo or open an issue and I'll add it.

## Disable Welding Rods
*Fix Source: me*

The inclusion of welding rods adds significant overhead to the ability to craft or modify vehicles. While the gameplay style encouraged by this feature is preferable to some, it removes the freedom to alter and experiment with vehicle designs without expending a limited resource. The removal of this freedom significantly changes the pacing of Cataclysm in a way that is irreconcileable between different kinds of players, and is thus best left optional.

### Option 1: Use a mod

Use the [no welding rods](https://github.com/neonspectra/no-welding-rods) mod to overwrite recipes to not require welding rods.

Downsides for this option:
- CDDA is goofy about recipe overwrites for existing recipes. I've noticed that sometimes the overwrites don't apply properly, particularly for vehicle-related part installations.
    - I have no idea why it's like this. If you make the same json edits to your base game instead of putting them in a mod, it works fine.

### Option 2: Manually edit json

1. Open `data/json/requirements/toolsets.json` in your text editor of choice.
2. Remove the `[ [ "welding_rod_steel", 1 ], [ "welding_wire_steel", 1 ], [ "brazing_rod_bronze", 1 ] ]` components from the following four IDs:
    - `welding_standard`
    - `welding_alloys`
    - `repair_welding_standard`
    - `repair_welding_alloys`
3. Note that JSON will get mad if you have a comma on the last item in a list. 
    - IE, if you delete the entire components line from `welding_standard` and `welding_alloys`, make sure to remove the comma at the end of `tools` or your game will crash when you try to launch.

Downsides for this option:
- You will have to re-apply this tweak every time you update your game.
