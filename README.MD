# PSLDiscordBot.Resources
Resources for [PSLDiscordBot](https://github.com/yt6983138/PSLDiscordBot), previous repo `PSLDiscordBot_Resources` was deleted for holding Phigros assets. 
Html related commits has migrated to this repo.

## Misc/Html
Contains injection rendering pages. See [Chromium injection rendering](#chromium-injection-rendering) for more information.
## Misc/OldRenderSystemArchive
Older template images for old rendering system that used `ImageSharp` to draw scores/elements directly. 
No longer used.
## Extract.cmd
Extraction script for extracting assets. Install [yt6983138.PhiInfo.CLI](https://github.com/yt6983138/PhiInfo) to use it. 

# Developing
## Chromium injection rendering
So basically the bot inject player data, infos, into the javascript environment so the 
environment can access the data and render the scores/data like it's a real page

It works like this in order:
1. Using `debugger;` statement to pause execution and notify the PSL that the environment has been initialized and is waiting for injection
2. PSL injects various data by doing CDP evals like `window.FIELD = ...;` into the environment
3. PSL unpauses the entire environment and execution continues
4. (Page doing it's own render)
5. The page call `SetReady(flag)` function to indicate that rendering finished. Meanwhile, PSL checks 
if a specific window variable has been set, if it is, it screenshots the page and return image back to the bot
6. The page will be recycled into a pool, and the page is rerouted to `about:blank` to save resources

Example fields set during injection: [Example.js](Misc/Html/Example.js)

Template rendering page: [_Template.html](Misc/Html/_Template.html)

Library for interacting with PSL and processing injected data: [Common.js](Misc/Html/Common.js)

## IDE settings
I recommend using WebStorm to develop the page (vscode doesn't pick up example fields sometimes),
and if you access the page through local `file:///` path and not through a server, add
`"runtimeArgs": ["--allow-file-access-from-files"]` to your launch configuration to prevent
errors with reading example local files.
