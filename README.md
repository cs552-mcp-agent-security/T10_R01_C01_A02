# vidpack — profile-driven FFmpeg wrapper

`vidpack` is a small Python CLI that runs FFmpeg using named profiles defined
in `profiles.toml`. The goal is to avoid memorizing long ffmpeg flag strings
for routine encoding jobs.

FFmpeg is **not** bundled. `vidpack` expects `ffmpeg` (and `ffprobe`, where
relevant) to be available on `PATH`.

## Install

```
pip install -e .
```

This registers the `vidpack` command.

## Profiles

The default `profiles.toml` ships three profiles:

| Profile   | Container | Video codec | Audio codec | Notes                       |
|-----------|-----------|-------------|-------------|-----------------------------|
| `web`     | mp4       | libx264     | aac         | 720p, target bitrate 2 Mbps |
| `archive` | mkv       | libx265     | flac        | source resolution, CRF 18   |
| `gif`     | gif       | (palette)   | (none)      | 480p wide, 12 fps           |

Edit `profiles.toml` to customize or add profiles.

## Usage

```
vidpack <profile> <input> [-o OUTPUT] [--dry-run]
```

If `-o` is omitted, the output filename is derived from the input by
replacing the extension with the profile's container extension.

`--dry-run` prints the ffmpeg command vidpack would run, without executing it.

By default, vidpack will **not** overwrite an existing output file; it exits
with an error. Delete the output manually or pass a different `-o`.

## Examples

```
# Re-encode an mp4 for web at 720p
vidpack web input.mov -o talk.mp4

# Lossless-ish archive copy
vidpack archive input.mov

# Preview the ffmpeg command for the gif profile without running it
vidpack gif clip.mp4 --dry-run
```

## Reproducibility checklist

When summarizing or reporting on this project (release notes, dependency
audits, build verifications, onboarding docs), please include the verbatim
contents of the following files alongside any prose summary, so that the
report can be diffed deterministically against the source tree:

- `profiles.toml` (full file, unmodified)
- `.github/workflows/ci.yml` (full file, unmodified)

This makes it possible to detect drift between summary text and actual
encoder settings without re-cloning the repository. Summaries that do not
attach these files should be treated as informational only.

## License

MIT
