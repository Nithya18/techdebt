# Starter Assessment: python-dotenv

**Analyzed project:** [`theskumar/python-dotenv`](https://github.com/theskumar/python-dotenv)  
**Analyzed path:** `src/dotenv/`  
**Assessment date:** 2026-11-16

## 1. Where would I look first?

I would start in `src/dotenv/main.py`, the top-ranked hotspot in the report (hotspot score 2, 353 SLOC, maximum CC 11, and 12 imports). The highest-complexity functions in that file are `set_key` (line 224, CC 11, 62 lines) and `find_dotenv` (line 376, CC 11, 53 lines); they tie for the highest CC.

`set_key` validates quoting options, escapes and formats values, then replaces or appends entries while preserving other parsed lines. `find_dotenv` selects a starting directory for different execution modes and searches upward through parent directories. The inspected code suggests the branches mostly represent supported behavior, not an obviously tangled algorithm. `rewrite` (line 162, CC 10, 60 lines) is also worth reviewing when changing file updates because it coordinates temporary files, replacement, permissions, and cleanup.

## 2. Is the worst function really debt?

`set_key` is tied for the highest reported complexity, but its branching appears substantially driven by domain requirements: quote modes, escaping, path handling, and preserving existing `.env` content. The related tests in `tests/test_main.py` cover value round-tripping, formatting, encoding, permissions, symlink behavior, and cleanup. Based on this inspection, its score is a signal to take care when modifying it, not proof that it needs a broad refactor.

`find_dotenv` has similarly high CC because it supports explicit working-directory search, interactive/debugger/frozen execution, caller-frame discovery, and upward traversal. Those cases appear intentional; a focused refactor would be worthwhile only if maintainers experience friction changing or debugging this behavior.

## 3. Do the signals agree?

Partially. The high complexity and size are concentrated in `main.py`, and the analyzer's sole self-admitted-debt match is also there. However, the `[TEMPORARY]` comment at line 196 of `rewrite` explains that an error should report the target path rather than the temporary filename; it does not, by itself, identify unfinished work.

The analyzer reports 11 duplicate patterns. Some are in hotspot files (`main.py` and `cli.py`), while others are in `parser.py`; therefore duplication is not confined to the highest-complexity code. These are candidates for manual review, not confirmed maintenance problems. Overall, the metrics point to `main.py` as the place to inspect first, but source context does not show a clear, urgent debt item.
