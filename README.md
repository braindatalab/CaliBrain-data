# CaliBrain example data

Example forward solutions for
[CaliBrain](https://github.com/braindatalab/CaliBrain), a Python toolbox for
uncertainty quantification, calibration, and benchmarking in Bayesian M/EEG inverse source imaging.

The files are published as release assets of this repository, not in git. Check the
[Releases](https://github.com/braindatalab/CaliBrain-data/releases) page.

## Contents

| Release | Archive | Description |
|---|---|---|
| `example-data-fsaverage-2` | `calibrain-example-data-fsaverage.zip` | `fsaverage` template: the forward solution (full source space) |
| `example-data-camcan-2` | `calibrain-example-data-camcan.zip` | forward solutions of four CamCAN participants (full source space; derived data, subject to the CamCAN terms, see below) |

Each archive unpacks into the folder `fwd`:

```text
fwd/fsaverage-fwd.fif                   MNE forward solution (free orientation)   [fsaverage]
fwd/CC120166-fwd.fif  ... CC120313-fwd.fif   one per participant (CC120166, CC120264,
                                        CC120309, CC120313)                       [camcan]
```

- The full source space of the cortical surface: 2562 sources per hemisphere (about 5100
  in total). `fsaverage` has 64 EEG channels; the CamCAN forwards have 306 MEG channels
  (102 magnetometers, 204 gradiometers).
- CaliBrain works with a reduced source space of 1284 sources (642 per hemisphere, a random
  selection with seed 42). The reduced forward solutions and the leadfield files are
  **not** included. Their format depends on the version of CaliBrain, so they are created
  from these files on your computer (see below). That way they always match the CaliBrain
  version you use, and these releases do not become outdated when the format changes.

## How to use

The easiest way is CaliBrain itself, which downloads and checks the data (three
independent steps):

```python
from calibrain import extract_leadfields, fetch_example_data, reduce_forward_solutions

fetch_example_data()         # step 1: download the forward solution
reduce_forward_solutions()   # step 2: reduce it to the 1284 sources CaliBrain uses
extract_leadfields()         # step 3: create the leadfield files from it
# fetch_example_data("camcan") downloads the CamCAN forwards (read the CamCAN terms below first)
```

By hand: download the archive from the release page, check it, and unpack it into a data
directory of your choice, then reduce and extract with that directory:

```bash
DATA=/path/to/calibrain_data
mkdir -p "$DATA"
curl -L -O https://github.com/braindatalab/CaliBrain-data/releases/download/example-data-fsaverage-2/calibrain-example-data-fsaverage.zip
sha256sum calibrain-example-data-fsaverage.zip     # compare with the checksum on the release page
unzip calibrain-example-data-fsaverage.zip -d "$DATA"
python -m calibrain.reduce_fwd --data-dir "$DATA"           # step 2: reduce to 1284 sources
python -m calibrain.extract_leadfield --data-dir "$DATA"    # step 3: create the leadfield files
```

## Where the data come from

- The forward solution was computed with MNE-Python's `make_forward_solution` for
  the `fsaverage` template (the `fsaverage` data distributed with MNE-Python,
  which originates from FreeSurfer). The source space has 2562 sources per
  hemisphere. CaliBrain reduces it to 642 per hemisphere (random selection with seed 42).
- The `fsaverage` data are computed from a template. They contain no participant data.
- The `camcan` forward solutions were computed from the MRI-based head model, source
  space and MEG sensor layout of four participants of the Cambridge Centre for Ageing and
  Neuroscience (Cam-CAN) data repository (participant IDs `CC120166`, `CC120264`,
  `CC120309`, `CC120313`). The source space has about 2562 sources per hemisphere. The files
  contain the source-space geometry and the sensor gain matrix,
  and no images and no participant information beyond the pseudonymous participant IDs.
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

**CamCAN-derived files.** The `camcan` dataset is derived from data of the Cam-CAN
repository, and its use is additionally subject to the
[CamCAN terms and conditions](https://opendata.mrc-cbu.cam.ac.uk/projects/camcan/). If you
use these files, acknowledge CamCAN and cite the CamCAN publications as described on the
CamCAN data portal. The files do not replace requesting access to the CamCAN data if you
need the original recordings. Do not attempt to identify any participant.

## Citation

If you use these data, please cite CaliBrain
(<https://doi.org/10.5281/zenodo.20683761>, the DOI that always points to the latest
version). The `CITATION.cff` file in this repository contains the citation
information.

## Versions and corrections

Each release is immutable: a changed dataset gets a new release (`example-data-fsaverage-3`,
...). Files are never replaced in an existing release, so the checksums in
CaliBrain stay valid. The releases `example-data-fsaverage-1` and `example-data-camcan-1`
contain the same forward solutions already reduced to 1284 sources (in the folder
`1284src_fwd`). They are superseded by the `-2` releases and stay online for earlier
versions of CaliBrain. If you find a problem, please open an issue in this
repository.
