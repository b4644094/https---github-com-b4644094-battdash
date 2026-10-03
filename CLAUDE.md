# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

This is the unmodified `streamlit hello` scaffold app (the demo you get from running `streamlit hello`), checked in as its own repo. Despite the repo name (`battdash`), no battery-dashboard functionality has been added yet — every file is Streamlit's stock example content. Treat this as a starting template: the real application is expected to be built on top of it.

## Commands

Install dependencies:
```bash
pip install -r requirements.txt
```

Run the app locally:
```bash
streamlit run Hello.py
```
The app serves on port 8501. In the devcontainer, it is launched with CORS/XSRF protection disabled:
```bash
streamlit run Hello.py --server.enableCORS false --server.enableXsrfProtection false
```

There is no lint, test, or build tooling configured in this repo (no test framework, linter, or CI config present).

## Architecture

Streamlit's [multipage app](https://docs.streamlit.io/library/get-started/multipage-apps) convention drives the structure:

- `Hello.py` is the entrypoint page (run via `streamlit run Hello.py`).
- `pages/` holds additional pages; Streamlit auto-discovers and orders them in the sidebar by the leading number in each filename (`0_Animation_Demo.py`, `1_Plotting_Demo.py`, `2_Mapping_Demo.py`, `3_DataFrame_Demo.py`). New pages should follow this `N_Name.py` naming pattern to control sidebar order.
- Each page in `pages/` is a standalone script: it calls `st.set_page_config(...)`, renders a header, defines a `*_demo()` function containing the page's logic, then calls that function directly at module scope followed by `show_code(...)` to display the function's own source in an expandable "Show code" panel.

**Known quirk:** every file in `pages/` imports `show_code` from `streamlit.hello.utils` (Streamlit's own internal package path), not from the local `utils.py` at the repo root. The root-level `utils.py` (which defines the same `show_code` function) is currently dead code — no page imports it. If you add new pages or refactor `show_code`, decide whether to keep relying on Streamlit's internal module or switch pages to import from the local `utils.py`.

`__init__.py` at the repo root is empty (license header only) and exists just to make the directory importable.
