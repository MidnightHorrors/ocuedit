# OcuEdit

### Project Goals

OcuEdit is a web-based wavedata editor for
[Oculus' Game](https://www.roblox.com/games/6165472594/Oculus-Game). You can
view a live version of OcuEdit on
[the Midnight Horrors website](https://ocuedit.midnight-horrors.com/). This
project should:

- Be able to edit new wavedata strings.
- Be able to export and import wavedata strings.
- Be able to save your wavedata locally.
- Be able to load your wavedata from a file.

This README includes the [wavedata specification](#wavedata-specification)
provided by Blithwin.

### Running Locally

To run the project locally, you will need to have
[Deno](https://deno.land/manual@v1.29.1/getting_started/installation) installed.
You can install Deno in one line using the following command:

```
curl -fsSL https://deno.land/x/install/install.sh | sh
```

Once Deno is installed, you can run the project using the following command:

```
deno task start
```

This will start the project on port 8000. You can access the project at
`localhost:8000`.

### Contributing

If you would like to contribute to this project, you can do so by creating a
pull request. Please make sure that your code is formatted using `deno fmt`. For
more information, read the [CONTRIBUTING.md](CONTRIBUTING.md).

### Wavedata Specification

I have created an EBNF to represent the specification. This EBNF has complete
type checking and is able to validate any wavedata. You can view it on the
[docs]() or see it below.

```ebnf
(* Oculus' Game Wavedata *)
Round = "{" (Wave ("," Wave)*)? "}";
Wave  = "{" (Function ("," Function)*)? "}";

(* Literals *)
Number     = [0-9]+ | "{" RandomNum "}";
Identifier = [a-zA-Z_][a-zA-Z0-9_]*;
Function   = "{" Call "}";
Array      = "{" Identifier ("," Identifier)* "}";
Call       = ChangeDiff | SpawnKiller | SpawnItem | ChangeMusic |
             Repeat | RandomNum | ChangeWaveTime | SetBlacklist
             SetWhitelist | GetCurrentDiff;

(* Enums *)
DiffType   = "Easy" | "Medium" | "Hard" |
             "Impossible" | "???" | "{" GetCurrentDiff "}";
Location   = "BSP" | "Docks" | "Even Care" |
             "Funhouse" | "Gloom" | "Rocket Arena" |
             "Rooftops" | "Space Cylinder";
ItemName   = "Bat" | "Blue relic" | "Booz Berry" | "Egg Tube" |
             "FreezeRay" | "FriendsCoil" | "GlassCoil" | "Hcoil" |
             "Noob Tube" | "PipeBomb" | "Pumpkin Staff" | "PurpleCoil" |
             "Radar" | "Regen Coil" | "Speed Coil" | "Ultra Coil" |
             "Wormhole scissors" | "Yolkster Plant" | "picklerick" |
             "sall" | "yolkster gun";
List       = "Killers" | "Music" | "ItemSpawn" | "Items" | "KillerSpawns";
KillerName = (* All killers in Oculus' Game *) | "{" GetCurrentDiff "}";

(* Parameters *)
ChangeDiffParams     = "," DiffType;
SpawnKillerParams    = ("," KillerName | DiffType ("," Location)? )?;
SpawnItemParams      = ("," ItemName ("," Number)? )?;
ChangeMusicParams    = ("," (Identifier | DiffType))?;
RandomNumParams      = "," Number "," Number;
ChangeWaveTimeParams = "," Number;
SetBlacklistParams   = "," List "," Array;
SetWhitelistParams   = "," List "," Array;
RepeatParams         = "," RepeatBody;

(* Functions *)
ChangeDiff     = "ChangeDiff" ChangeDiffParams;
SpawnKiller    = "SpawnKiller" SpawnKillerParams;
SpawnItem      = "SpawnItem" SpawnItemParams;
ChangeMusic    = "ChangeMusic" ChangeMusicParams;
RandomNum      = "RandomNum" RandomNumParams;
ChangeWaveTime = "ChangeWaveTime" ChangeWaveTimeParams;
SetBlacklist   = "SetBlacklist" SetBlacklistParams;
SetWhitelist   = "SetWhitelist" SetBlacklistParams;
GetCurrentDiff = "GetCurrentDiff";
Repeat         = "Repeat" RepeatParams;

(* Repeat Management *)
ChangeDiffRepeat     = "ChangeDiff" "," Number "," ChangeDiffParams;
SpawnKillerRepeat    = "SpawnKiller" "," Number ("," SpawnKillerParams)?;
SpawnItemRepeat      = "SpawnItem" "," Number ("," SpawnItemParams)?;
ChangeMusicRepeat    = "ChangeMusic" "," Number ("," ChangeMusicParams)?;
RandomNumRepeat      = "RandomNum" "," Number "," RandomNumParams;
ChangeWaveTimeRepeat = "ChangeWaveTime" "," Number "," ChangeWaveTimeParams;
SetBlacklistRepeat   = "SetBlacklist" "," Number "," SetBlacklistParams;
SetWhitelistRepeat   = "SetWhitelist" "," Number "," SetWhitelistParams;
GetCurrentDiffRepeat = "GetCurrentDiff" "," Number;
RepeatRepeat         = "Repeat" "," Number "," RepeatParams;

RepeatBody           = ChangeDiffRepeat | SpawnKillerRepeat | ChangeMusicRepeat | 
                       SpawnItemRepeat | RepeatRepeat | RandomNumRepeat |
                       ChangeWaveTimeRepeat | SetBlacklistRepeat | SetWhitelistRepeat |
                       GetCurrentDiffRepeat;
```

Blithwin has also created images to summarize the creation of wavedata. You can
view these images below.
![Functions](https://cdn.discordapp.com/attachments/794746051306586123/869713568419938374/unknown.png)
![Format](https://cdn.discordapp.com/attachments/794746051306586123/869726884156538940/unknown.png)

### License

This project and all other projects under this organization are licensed under
the MIT license. You can view the license [here](LICENSE.md).
