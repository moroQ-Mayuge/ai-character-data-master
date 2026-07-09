# AI Character Data Master

## Overview
This repository maintains normalized master data for anime, game, manga and related characters for use with image generation AI and LLM applications.

Primary goals:
- Normalize character names.
- Verify Danbooru character tags.
- Identify original works.
- Organize character information.
- Provide master data for future JSON/CSV exports.

## Documentation
Detailed specifications are available under `docs/`.

- `docs/research_policy.md` - Research policy, workflow, web access policy, data structure and automation.
- `character-master-data/docs/character_gacha_spec.md` - Character information format and gacha specification.

## Repository Structure
- `data/org_charlist.csv` : Original research source (do not edit directly)
- `docs/` : Documentation
- `exports/` : Generated datasets

## Basic Policy
- Keep the original source unchanged.
- Store research results separately.
- Do not confirm uncertain information by guesswork.
- Keep references and confidence.
- Respect website policies and perform only low-frequency, minimal web access.

See `docs/research_policy.md` for details.

## License
Character names and copyrights belong to their respective rights holders. This repository is intended as a structured research database.