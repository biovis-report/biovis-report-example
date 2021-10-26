# Example

## Step1: Clone the example repo

```
git clone https://github.com/biovis-report/biovis-report-example.git
```

## Step2: Configuration

Install all the dependencies.

### Create an new environment

```bash
conda create -n biovis-report python=3.8
```

### Activate the conda environment

```bash
conda activate biovis-report
```

### Install the core packages (pip/conda)

```bash
# use pip
conda activate biovis-report
pip3 install biovis-report biovis-media-extension

# use conda
conda install -c biovis-report -c conda-forge biovis-report biovis-media-extension
```

### Test whether the biovis-report is working.

```bash
biovis-report --help
```

### Install the plugins

```bash
# Base plugin package
conda install -c biovis-report -c conda-forge biovis-base-plugins

# More plugins
conda install -c biovis-report -c conda-forge <plugin-name>
```

### Launch the biovis-report development server

```bash
biovis-report report -p ./output -t ./example -e -f --theme biovis_mkdocs --dev-addr 0.0.0.0:8023
```

## Step3: Open your chrome browser and access the [http://localhost:8023](http://localhost:8023)