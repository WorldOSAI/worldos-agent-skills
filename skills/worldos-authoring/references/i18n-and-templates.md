# Localization and template variables

## Locale overlays

Use `i18n[locale]` as a generic overlay that mirrors the canonical structure. Do not introduce language-specific field names.

The canonical content can be written in any language. English is not a privileged read path: when an English overlay exists, it must be eligible just like any other locale.

For arrays of localizable objects:

- give every item a stable `id`;
- align overlay entries by `id`, not by translated label;
- preserve identifiers, enum values, URLs, colors, and numeric data;
- localize player-visible names, descriptions, messages, labels, prompts, and suggestions;
- write natural product copy for the locale rather than literal sentence-by-sentence translation.

Treat legacy fields ending in language suffixes as migration debt, never as examples for new content.

Do not treat `config.localization.readyLocales` as proof that translation exists. The owned-world response exposes a read-only `localizationStatus` computed from actual world and installed-App overlays. If it reports missing fields or inconsistent readiness mirrors, how to respond depends on the translation threshold.

Automatic `en`/`es`/`zh` translation is earned: WorldOS translates a world once its total play turns reach the site's translation threshold (currently 1000 turns). Below the threshold a world stays single-language, and missing overlays are expected rather than a defect.

- **At or above the threshold, or an official world:** call `request_world_localization` with the exact world version and re-fetch until the repair completes. The request does not publish the world or change its visibility.
- **Below the threshold:** `request_world_localization` is refused. If the creator needs other languages before then, write the overlays yourself in the world's `config.i18n[locale]` and in each installed App's `config.i18n[locale]`, following the rules above. Authored overlays are preserved by later writes and by automatic translation once the world qualifies.

## Player variables

Use the standard semantic setup fields when appropriate:

- `player_name` for the player’s name;
- `player_persona` for background, identity, or play style.

Reference their values with the template syntax supported by the current WorldOS contract. Give required fields useful defaults and a few clickable options so the Simulation can start without typing.

## Character variables

Give customizable characters a stable ID and, when supported, a readable variable key. Reference the character through its template variable everywhere the character appears:

- world prompt;
- character descriptions and relationships;
- story opening;
- chat names and messages;
- posts and comments;
- action suggestions;
- widget initial data;
- map factions or markers.

Keep the character’s canonical `name` as the default value. Replace references to that name with the variable, not the name field itself.

## Reference integrity

Localization must not change identifiers. After composing every locale, verify that:

- character IDs remain identical;
- author and sender IDs resolve;
- faction `charId` values resolve;
- region and owner IDs resolve;
- localized arrays retain the same stable item IDs;
- template variables are not translated or partially rewritten.
