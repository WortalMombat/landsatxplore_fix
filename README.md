[![Build](https://img.shields.io/github/workflow/status/yannforget/landsatxplore/Upload%20Python%20Package?label=build&logo=github)](https://github.com/yannforget/landsatxplore/actions/workflows/python-publish.yml)
[![Tests](https://img.shields.io/github/workflow/status/yannforget/landsatxplore/Run%20tests?label=tests&logo=github)](https://github.com/yannforget/landsatxplore/actions/workflows/run-tests.yml)
[![codecov](https://codecov.io/gh/yannforget/landsatxplore/branch/master/graph/badge.svg?token=NwVo09Edur)](https://codecov.io/gh/yannforget/landsatxplore)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1291422.svg)](https://zenodo.org/record/4543601)

# Fork of landsatxplore to fix bug which prevents download of older data
Due to hardcoded download links, when downloading older C2-L2 data an error is raised: `landsatxplore.errors.EarthExplorerError: Download is not available`. This is documented [here](https://github.com/yannforget/landsatxplore/issues/42) and [here](https://github.com/yannforget/landsatxplore/issues/45) and a solution is proposed. This solution is implemented in this fork with the aim to make the fixed package available to download until a permanent solution is introduced by the original author(s) of landsatxplore.

*Update (Jan 11, 2023):* Dataset IDs in the USGS API have been changed again on Oct 19, 2022, which lead again to the abovementioned download errors. With the newest commit, I updated the Dataset IDs again, as documented [here](https://github.com/yannforget/landsatxplore/issues/92). New IDs can be retrieved from the test einvironment of the [Machine-to-machine API](https://m2m.cr.usgs.gov/api/test/json/). Select the Endnode ``dataset-download-options`` and submit the desired dataset name (e.g. ``landsat_ot_c2_l2``). The ID will show as "productID" in the response.

*Another fix implemented:* In the beginning another error regarding the log-in tokens on Earth Explorer was discovered and [described](https://github.com/yannforget/landsatxplore/issues/76). The user was able to solve the issue by changing some lines in the [earthexplorer.py](https://github.com/yannforget/landsatxplore/pull/75/files). This fix is also nor implemented in this fork. As of now (April 2022) the fix has yet to be merged into the original landsatxplore by the original author(s).

*Added compatability for Landsat 9 C2L2:* Compatability for the download of LS9 C2L2 was implemented as proposed by @faendeg in [this pull request](https://github.com/yannforget/landsatxplore/pull/69).

## Installation of landsatxplore_fix
Delete any previous installation of landsatxplore using ``pip uninstall landsatxplore``.

Install the fixed version using ``pip install git+https://github.com/WortalMombat/landsatxplore_fix.git``

# Description

![CLI Demo](https://raw.githubusercontent.com/yannforget/landsatxplore/master/demo.gif?s=0.5)

The **landsatxplore** Python package provides an interface to the [EarthExplorer](http://earthexplorer.usgs.gov/) portal to search and download [Landsat Collections](https://landsat.usgs.gov/landsat-collections) scenes through a command-line interface or a Python API.

The following datasets are supported:


| Dataset Name | Dataset ID |
|-|-|
| Landsat 5 TM Collection 1 Level 1 | `landsat_tm_c1` |
| Landsat 5 TM Collection 2 Level 1 | `landsat_tm_c2_l1` |
| Landsat 5 TM Collection 2 Level 2 | `landsat_tm_c2_l2` |
| Landsat 7 ETM+ Collection 1 Level 1 | `landsat_etm_c1` |
| Landsat 7 ETM+ Collection 2 Level 1 | `landsat_etm_c2_l1` |
| Landsat 7 ETM+ Collection 2 Level 2 | `landsat_etm_c2_l2` |
| Landsat 8 Collection 1 Level 1 | `landsat_8_c1` |
| Landsat 8 Collection 2 Level 1 | `landsat_ot_c2_l1` |
| Landsat 8 Collection 2 Level 2 | `landsat_ot_c2_l2` |
| Landsat 9 Collection 2 Level 1 | `landsat_ot_c2_l1` |
| Landsat 9 Collection 2 Level 2 | `landsat_ot_c2_l2` |
| Sentinel 2A | `sentinel_2a` |


# Quick start

Searching for Landsat 5 TM scenes that contains the location (12.53, -1.53) acquired during the year 1995.

```
landsatxplore search --dataset LANDSAT_TM_C1 --location 12.53 -1.53 \
    --start 1995-01-01 --end 1995-12-31
```

Search for Landsat 7 ETM scenes in Brussels with less than 5% of clouds. Save the returned results in a `.csv` file.

```
landsatxplore search --dataset LANDSAT_ETM_C1 \
    --location 50.83 4.38 --clouds 5 > results.csv
```

Downloading three Landsat scenes from different datasets in the current directory.

```
landsatxplore download LT51960471995178MPS00 LC80390222013076EDC00 LC82150682015350LGN01
```

To use the package, Earth Explorer credentials are required ([registration](https://ers.cr.usgs.gov/register)).

# Installation

The package can be installed using pip.

```
pip install landsatxplore
```

# Usage

**landsatxplore** can be used both through its command-line interface and as a Python module.

## Command-line interface

```
landsatxplore --help
```

```
Usage: landsatxplore [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  download  Download one or several Landsat scenes.
  search    Search for Landsat scenes.
```

### Credentials

Credentials for the Earth Explorer portal can be obtained [here](https://ers.cr.usgs.gov/register/).

`--username` and `--password` can be provided as command-line options or as environment variables:

``` shell
export LANDSATXPLORE_USERNAME=<your_username>
export LANDSATXPLORE_PASSWORD=<your_password>
```

### Searching

```
landsatxplore search --help
```

```
Usage: landsatxplore search [OPTIONS]

  Search for Landsat scenes.

Options:
  -u, --username TEXT             EarthExplorer username.
  -p, --password TEXT             EarthExplorer password.
  -d, --dataset [landsat_tm_c1|landsat_etm_c1|landsat_8_c1|landsat_tm_c2_l1|landsat_tm_c2_l2|landsat_etm_c2_l1|landsat_etm_c2_l2|landsat_ot_c2_l1|landsat_ot_c2_l2|sentinel_2a]
                                  Landsat data set.
  -l, --location FLOAT...         Point of interest (latitude, longitude).
  -b, --bbox FLOAT...             Bounding box (xmin, ymin, xmax, ymax).
  -c, --clouds INTEGER            Max. cloud cover (1-100).
  -s, --start TEXT                Start date (YYYY-MM-DD).
  -e, --end TEXT                  End date (YYYY-MM-DD).
  -o, --output [entity_id|display_id|json|csv]
                                  Output format.
  -m, --limit INTEGER             Max. results returned.
  --help                          Show this message and exit.
```

### Downloading

```
landsatxplore download --help
```

```
Usage: landsatxplore download [OPTIONS] [SCENES]...

  Download one or several Landsat scenes.

Options:
  -u, --username TEXT    EarthExplorer username.
  -p, --password TEXT    EarthExplorer password.
  -d, --dataset TEXT     Dataset.
  -o, --output PATH      Output directory.
  -t, --timeout INTEGER  Download timeout in seconds.
  --skip
  --help                 Show this message and exit.
```

If the `--dataset` is not provided, the dataset is automatically guessed from the scene identifier. Note that only the newer Landsat Product Identifiers contain information related to collection number and processing level. To download Landsat Collection 2 products, use Product IDs or set the `--dataset` option correctly.


## API

### EarthExplorer API

**landsatxplore** provides an interface to the Earth Explorer JSON API. Please refer to the official ([documentation](https://earthexplorer.usgs.gov/inventory/documentation/json-api)) for possible request codes and parameters.

#### Basic usage

``` python
from landsatxplore.api import API

# Initialize a new API instance and get an access key
api = API(username, password)

# Perform a request. Results are returned in a dictionnary
response = api.request(
    '<request_endpoint>',
    params={
        "param_1": value_1,
        "param_2": value_2
    }
)

# Log out
api.logout()
```

Please refer to the official [JSON API Reference](https://m2m.cr.usgs.gov/api/docs/json/) for a list of all available requests.

#### Searching for scenes

``` python
import json
from landsatxplore.api import API

# Initialize a new API instance and get an access key
api = API(username, password)

# Search for Landsat TM scenes
scenes = api.search(
    dataset='landsat_tm_c1',
    latitude=50.85,
    longitude=-4.35,
    start_date='1995-01-01',
    end_date='1995-10-01',
    max_cloud_cover=10
)

print(f"{len(scenes)} scenes found.")

# Process the result
for scene in scenes:
    print(scene['acquisition_date'].strftime('%Y-%m-%d'))
    # Write scene footprints to disk
    fname = f"{scene['landsat_product_id']}.geojson"
    with open(fname, "w") as f:
        json.dump(scene['spatial_coverage'].__geo_interface__, f)

api.logout()
```

Output:

```
4 scenes found.
1995-09-23
1995-08-22
1995-08-15
1995-06-28
```

#### Downloading scenes

``` python
from landsatxplore.earthexplorer import EarthExplorer

ee = EarthExplorer(username, password)

ee.download('LT51960471995178MPS00', output_dir='./data')

ee.logout()
```
