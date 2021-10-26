# Example

## Step1: Clone the example repo

```
git clone https://github.com/biovis-report/biovis-report-example.git
```

## Step2: Configuration

```
# Install all the dependencies
conda create -n biovis-report python=3.7
conda install -c biovis-report biovis-report biovis-media-extension biovis-base-plugins

# Activate the conda environment
conda activate biovis-report

# Test whether the biovis-report is working.
biovis-report --help

# Launch the biovis-report development server
biovis-report report -p ./output -t ./example -e -f --theme biovis_mkdocs --dev-addr 0.0.0.0:8023
```

## Step3: Open your chrome browser and access the [http://localhost:8023](http://localhost:8023)