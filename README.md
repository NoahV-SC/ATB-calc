# Annual Technology Baseline Electricity Data Processing Pipeline

Python files and Jupyter notebooks for processing the Annual Technology Baseline (ATB) electricity data and determining LCOE and other metrics. All documentation and data for the ATB is available at the [ATB website](https://atb.nrel.gov).

## Installation and Requirements
The pipeline requires [Python](https://www.python.org) 3.10 or newer. Dependancies can be installed using `pip`: **TODO possibly include note about installing jupyter**


```
$ pip install -r requirements.txt
```

Once Python and all dependancies are installed the installation can be tested by running:

```
$ pytest
```

Tests take about a minute and should complete without errors. The ATB pipeline uses [xlwings](https://www.xlwings.org/) for accessing the ATB data spreadsheet and requires a copy of Microsoft Excel. Currently the full pipeline will only run on MacOS and Windows. Linux support may be added in the future.

## Running the ATB Electricity Pipeline
Running the pipeline requires downloaded the most current data in `xlsx` format from the
[ATB website](https://atb.nrel.gov). The pipeline may be ran for one or all ATB electricity technologies.
Data may be exported in several formats. Below are several example workflows. It is assumed that all
commands are run from the root directory of the repository. In the examples `{PATH-TO-DATA-MASTER}`
is the path and filename to the ATB electricity data master `xlsx` file.

Process all techs and export to a flat file named `flat_file.csv`:

```
$ python -m lcoe_calculator.full_scrape --save-flat flat_file.csv {PATH-TO-DATA-MASTER}
```

Process only land-based wind and export pivoted data and meta data:

```
$ python -m lcoe_calculator.full_scrape --tech LandBasedWindProc \
	--save-pivoted pivoted_file.csv --save-meta meta_file.csv {PATH-TO-DATA-MASTER}
```

Process only pumped storage hydropower and copy data to the clipboard so it may be pasted into a spreadsheet:

```
$ python -m lcoe_calculator.full_scrape --tech PumpedStorageHydroProc \
	--clipboard {PATH-TO-DATA-MASTER}
```

Help for the scraper and the names of available technologies can be viewed by running:

```
$ python -m lcoe_calculator.full_scrape --help
```

## Debt Fraction Calculator
The debt fraction calculator uses [PySAM](https://nrel-pysam.readthedocs.io/en/main/) to calculate
debt fractions for one or all ATB technologies. To calculate debt fractions for all technologies run
the following from the repository root directory:

```
$ python -m  debt_fraction_calculator.debt_fraction_calc {PATH-TO-DATA-MASTER} \
	{OUTPUT-CSV-FILE}
```

where `{PATH-TO-DATA-MASTER}` is the path and filename of the ATB data master spreadsheet, and
`{OUTPUT-CSV-FILE}` is the name of the `csv` file to create with the calculated debt fractions.

Debt fractions can also be calculated for a single ATB technology if desired. The below command will
calculate debt fractions for land-based wind:

```
$ python -m debt_fraction_calculator.debt_fraction_calc --tech LandBasedWindProc \
	{PATH-TO-DATA-MASTER} {OUTPUT-CSV-FILE}
```

All options for the debt calculator can be viewed with:

```
$ python -m debt_fraction_calculator.debt_fraction_calc --help
```

## Example Jupyter Notebooks
The `./example_notebooks` directory has several [Jupyter](https://jupyter.org/) notebooks showing
how to perform various tasks. The notebooks are a good way to understand how to use the code and
experiment with the ATB pipeline code. Jupyter must first be installed before use:

```
$ pip install jupyter
```

The Jupyter server can then be started by running:

```
$ jupyter-notebook
```

in the root repository directory.

## Notable Directories
- `./lcoe_calculator` Extract technology metrics from the data master `xlsx` file and calculate LCOE
using Python.
- `./debt_fraction_calculator` Given data and assumptions in the data master xlsx file, calculate
debt fractions using PySAM.
- `./example_notebooks` Example Jupyter notebooks showing how to perform various operations.
- `./tests` Tests for code in this repository.
