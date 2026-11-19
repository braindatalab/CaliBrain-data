# CaliBrain example data

Example forward solutions and leadfields for
[CaliBrain](https://github.com/braindatalab/CaliBrain), a Python toolbox for
uncertainty quantification and calibration in EEG/MEG inverse source imaging.

The files are published as release assets of this repository, not in git. Check the
[Releases](https://github.com/braindatalab/CaliBrain-data/releases) page.

## Contents

| Release | Archive | Description |
|---|---|---|
| `example-data-1` | `calibrain-example-data-fsaverage.zip` | `fsaverage` template: forward solution and fixed/free leadfields |

The archive unpacks into two folders:

```text
1284src_fwd/fsaverage-fwd.fif                    MNE forward solution (free orientation)
1284src_leadfield/fsaverage_fixed_leadfield.npz  fixed-orientation leadfield
1284src_leadfield/fsaverage_free_leadfield.npz   free-orientation leadfield
```

- 1284 sources (642 per hemisphere) on the cortical surface, 64 EEG channels.
- Leadfield shapes: fixed `(64, 1284)`, free `(64, 1284, 3)`.
- Every leadfield file also contains `cortical_normals` `(1284, 3)`, `Q_basis`,
  `src_coords`, and the sensor information. See the CaliBrain documentation for
  their meaning.

## How to use

CaliBrain reads the data from the directory in the `CALIBRAIN_DATA` environment
variable. Download the archive from the release page, check it, and unpack it
there:

```bash
export CALIBRAIN_DATA=/path/to/calibrain_data
mkdir -p "$CALIBRAIN_DATA"
curl -L -O https://github.com/braindatalab/CaliBrain-data/releases/download/example-data-1/calibrain-example-data-fsaverage.zip
sha256sum calibrain-example-data-fsaverage.zip     # compare with manifest.json on the release page
unzip calibrain-example-data-fsaverage.zip -d "$CALIBRAIN_DATA"
```

## Where the data come from

- The forward solution was computed with MNE-Python's `make_forward_solution` for
  the `fsaverage` template (the `fsaverage` data distributed with MNE-Python,
  which originates from FreeSurfer). The source space has 2562 sources per
  hemisphere, reduced to 642 per hemisphere (random selection with seed 42).
- The leadfield files were extracted from the forward solution with
  `python -m calibrain.extract_leadfield`.
- The data are computed from a template. They contain no participant data.
- Local file paths and user names were removed from all files.
- The archives are built with `scripts/build_example_data_archives.py` from the
  CaliBrain repository. Each release lists the SHA-256 checksum of every archive
  in `manifest.json`.

These data are provided to help users become familiar with CaliBrain. They are not
meant for evaluating any EEG, MEG, MRI, or forward-modeling system. See also the
`DATA_NOTICE` in the CaliBrain repository.

## License

The data in this repository are licensed under
[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
The `fsaverage` template comes from other projects (MNE-Python and FreeSurfer).
Please also consult their licenses.

## Citation

If you use these data, please cite CaliBrain
(<https://doi.org/10.5281/zenodo.20683761>, the DOI that always points to the latest
version). The `CITATION.cff` file in this repository contains the citation
information.

## Versions and corrections

Each release is immutable: a changed dataset gets a new release (`example-data-2`,
...). Files are never replaced in an existing release, so the checksums in
CaliBrain stay valid. If you find a problem, please open an issue in this
repository.
