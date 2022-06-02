# Example

Please access the [BioVisReport Docs Website](https://biovis.report) for more details

## Step1: Clone the example repo

```
git clone https://github.com/biovis-report/biovis-report-example.git

cd biovis-report-example
```

## Step2: Configuration

Install all the dependencies.

### Create an new environment

```bash
# It is recommended to install Python 3.8, and don't forget to specify the biovis-report and conda-forge channels please.
conda create -c biovis-report -c conda-forge -n biovis-report python=3.8 biovis-report
```

### Activate the conda environment

```bash
conda activate biovis-report
```

### Install the plugins

```bash
conda install -c biovis-report -c conda-forge biovis-rbased-plugins
conda install -c biovis-report -c conda-forge biovis-pybased-plugins
conda install -c biovis-report -c conda-forge biovis-jsbased-plugins
```

### Test whether the biovis-report is working.

```bash
biovis-report --help
```

### Launch the biovis-report development server

```bash
biovis-report report -p ./ -t ./example -e -f --theme biovis_mkdocs --dev-addr 0.0.0.0:8023
```

## Step3: Open your chrome browser and access the [http://localhost:8023](http://localhost:8023)
