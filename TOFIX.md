# TOFIX

Findings from a code scan on 2026-10-04.

## Medium

- `rsconstruct.toml:1` - the build only lints the workflow and `README.md`; nothing in this repo loads the term lists, so a term added to both directories, or twice in one directory (both are hard errors in rsconstruct's `load_terms`/`load_and_validate_terms`, `rsconstruct/src/processors/checkers/terms.rs:213` and `:254`), is only discovered when it breaks a consumer's build (teaching-syllabi, teaching-slides, business-syllabi, demos-os-linux). Add a `[processor.terms]` block pointing `dir_terms_unambiguous`/`dir_terms_ambiguous` at `unambiguous`/`ambiguous` with `src_files = ["README.md"]` so the registry validates itself.

## Low

- `config/project.lua:3` - `DESCRIPTION_SHORT` still describes the lists as "whitelist (single_meaning) and exclusion list (ambiguous)" and names only teaching-syllabi; the directories are now `unambiguous/` and `ambiguous/` (`README.md:9`) and there are four consumers. Update the description.
- `unambiguous/big_data_and_analytics.txt:18` - most term files are kept sorted but eight are not (`sort -c` fails at `big_data_and_analytics.txt:18`, `data_formats_and_standards.txt:18`, `file_systems_and_storage.txt:13`, `hardware_and_architecture.txt:4`, `ides_and_editors.txt:18`, `programming_constructs.txt:8`, `shells.txt:5`, `web_servers.txt:5`, all under `unambiguous/`), which makes spotting duplicates by eye harder. Sort them (`LC_ALL=C sort -o f f`).
