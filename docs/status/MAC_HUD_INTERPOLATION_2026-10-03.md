# Mac HUD: countdown and heart interpolation artifacts

At the Fire Mountain interior checkpoint (`MiniKaz`), 120-Hz Smooth Motion
could briefly draw a countdown digit between its timer slot and the rupee
counter on the right. Hearts could jump during their pulse/damage animation.

The matcher treated direct-position screen sprites as repeated instances of
one object, keyed by texture and occurrence. A reused glyph changed the draw
order: the trace paired a timer digit at `(412, 412)` with the counter and
placed its first in-between at `(585.2, 435.2)`. Occurrence is also unstable
when the heart animation adds or removes sprite copies.

Direct-position orthographic draws with depth testing and depth writes both
disabled now have no interpolation key. Every in-between replays their current
constants and vertex data intact. The HUD retains its original game-frame
animation; perspective world motion, water deformation, particles and
orthographic depth-buffer geometry keep their existing interpolation.

Patch `0140` records the HUD change and regression checks. The reproducible
Mac delta `0150`, selected and checksummed by `config/recompcore-patches.json`,
combines it with water `0113` and the Windows lava post-transform fix `0120`
on the unchanged pinned RecompCore base `8ab24da`.

Validation:

- Private copies of the player's checkpoint and memory card loaded before and
  after. Existing state layout and controller mode were preserved.
- Captured 40 game frames and their three in-betweens in the same scene. The
  fixed timer and rupee digits stay in their slots; the trace confirms the HUD
  draws have no key while the world still interpolates.
- The interpolation test passes, including shared-glyph UI draws, a stale
  particle tag on a HUD quad, depth-buffer geometry eligibility, world
  particles, CPU-deformed water, camera turns and 120-Hz pacing.
- The player confirmed the separate fixed test build looked corrected. The
  installed app was subsequently updated from that same native host, with
  every Mach-O section checked after signing and the latest checkpoint kept.

Private captures, logs, copied saves and installation backups live under
`build/wwhd-playtest/hud-interpolation/`; none are release or repository assets.
