# Official apps by genre

Use this table to decide what to search for. It names the official apps that usually carry each kind of world. It does not replace `search_apps` and `get_app_guide`: confirm every slug through search, read each live guide, and put opening state in each install's config.

`story` and `main-input` are the base of nearly every world and are not listed below. On top of them, install only the apps the core loop uses in its first few turns, each owning different facts. When the live guide for an app documents no config and the app needs seeded data to show anything, leave it out rather than guessing fields, unless this table gives the shape.

| World | Official apps to consider |
| --- | --- |
| "System" / LitRPG / cultivation / game transmigration | `system-notifications` (the system's pop-up voice; it holds no state, so missions still live in `quests`), `quests`, `equipment` (its progression levels can own the realm or level, and it carries a bag, so do not add `inventory` for the same items), `world-stats` for pressure values such as health or exposure, `achievements`; `wallet` to own a currency (one per world; a `shop` charges it automatically and ignores its own `currency` label when a wallet is installed, so a store priced in system points needs the wallet to be those points), `shop` for a system store the player uses early, `time` when rewards are daily |
| Romance / otome | `char-stats` (affection or trust per love interest), exactly one chat app (plain `chats`, or a skin: `wechat` for Chinese worlds, `messages-app`, `whatsapp` or `telegram` otherwise; all of them are mutually exclusive), `quests` for per-route arcs, `time` for the term or deadline; `dating` only when the story is about a dating app full of strangers; `visual-novel` only when you have sprite art, and it cannot be installed with `story` |
| Life sim | `world-stats` with a player opening, `time`, `achievements`; `social-feed`, `email-inbox`, `stock-ticker` or `shop` when the story uses them |
| Mystery / horror / containment / investigation | `chats` (interviewing people is the core action; it also carries in-person conversation), `evidence-board` (its live guide may be blank; it takes the Inventory guide's `lists: [{ id, label, items: [{ id, name, … }] }]` shape, which validation does not check, so copy it exactly), `email-inbox` for planted documents, `time` for an in-world deadline (not `timer`, which is a real-time countdown), `char-stats` for each suspect's own trust in or wariness toward the investigator (not the investigator's suspicion of them) |
| Fantasy adventure / tabletop | `dnd-map`, `dice`, `equipment`, `quests`, `shop` |
| Ruler / court / grand strategy | `decisions` (the GM files petitions to rule on), `dynasty` (it seeds one fixed family, so when the player may pick any realm, leave it unseeded or narrow the choice), `quests`, `world-stats` for the few national values the player steers; a region `map` for territory. A polity-scale war world follows `worldos-war-authoring` first; this row supplements it |
| Idol / streamer / influencer | `livestream`, `live-chat`, `social-feed`, `x-twitter` or `tiktok` |
| Business / management | `world-stats` for the few numbers the player steers, `shop` or `stock-ticker`, `email-inbox`, `time` |

Any world can add `achievements` for milestones, and `theater` for side stories that surface during play.

## Numbers the GM must keep

Do not add stats that the game master has to update every turn without a rule that moves them, such as garrison sizes, granaries, or army strength in a sandbox war world. They drift or freeze. Keep those in narration. Give every tracked number a stated rule in its install prompt, and keep tracked numbers to the few the player acts on.
