# Multi Sculpt & Paint

A Blender add-on for sculpting, vertex painting and weight painting **many
mesh objects at the same time** — the way the "LOL Poly" style multi-edit
workflows do it. Blender normally locks sculpt and paint modes to a single
active object; this add-on removes that limit.

Tested on **Blender 5.2 LTS** (built for 4.2 and newer).

## How it works

When you start a session the add-on:

1. Makes a copy of every selected mesh object (originals are never touched).
2. Stamps each copy with hidden tracking attributes so every vertex, edge,
   face and corner remembers which object and which element it came from.
3. Joins the copies into one temporary **session object** and drops you into
   Sculpt / Vertex Paint / Weight Paint on it.

You sculpt or paint freely across all of them as one object. When you press
**Apply & Finish**, the result is written back onto each original mesh element
by element — positions, colour attributes, vertex groups (weights), sculpt
masks and face sets all travel home — and the session object is deleted.

Because the mapping is explicit, write-back never depends on vertex order and a
topology change is **detected rather than silently corrupting your meshes**.

## Installing

1. In this folder, zip the `multi_sculpt_paint` directory (or use the
   prebuilt `multi_sculpt_paint.zip`).
2. Blender ▸ Edit ▸ Preferences ▸ Add-ons ▸ the ▾ menu ▸ *Install from Disk…*
3. Pick the zip and tick the checkbox to enable it.

## Using it

- Select **two or more mesh objects** (the active object is the one the session
  inherits its transform from).
- Open the **Multi** tab in the 3D Viewport sidebar (press <kbd>N</kbd>), or use
  the Object / Sculpt / Paint menus.
- Click **Multi Sculpt**, **Multi Vertex Paint** or **Multi Weight Paint**.
- Work as usual. Use the small mode buttons in the panel to switch between
  sculpt / vertex paint / weight paint without losing the session.
- **Back to Session Object** re-selects the merged object if you clicked away.
- **Apply & Finish** writes everything back; **Discard** throws the session
  away and leaves your originals exactly as they were.

While a session runs, a red **Multi …** badge sits in the viewport header.

## Rules and limits

- **Do not change topology** during a session — no Dyntopo, Remesh, Decimate,
  or adding/deleting geometry. Those change element counts, so the result can't
  be mapped back. Apply refuses the whole write in that case and keeps the
  session object so you can recover by hand (Add‑on Preferences ▸ *Allow Partial
  Apply* relaxes this to "write back whatever still matches").
- Objects must be **single-user** meshes. Linked duplicates are refused with a
  hint to make them single user.
- **Shape keys** are off by default (only the base mesh could round-trip);
  enable *Allow Shape Keys* in preferences to override.
- **Multiresolution** modifiers can't be merged and are refused.
- Modifiers on the active object are dropped from the session by default so one
  object's Mirror/Subdivision doesn't apply to everything; *Keep Modifiers of
  the Active Object* in preferences changes that.

## Preferences

| Setting | Default | What it does |
| --- | --- | --- |
| Hide Originals During Session | on | Hide the source objects while editing |
| Keep Modifiers of the Active Object | off | Keep the active object's modifier stack |
| Allow Shape Keys | off | Let keyed meshes join (base + Basis only) |
| Write Vertex Positions | on | Copy sculpted shape back |
| Write Colour Attributes | on | Copy vertex paint back |
| Write Vertex Groups | on | Copy weights back |
| Write Other Attributes | on | Copy masks, face sets, creases back |
| Allow Partial Apply | off | Write matching objects even if one broke |
| Confirm Before Discarding | on | Ask before discarding a session |
