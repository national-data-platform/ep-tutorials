# Complete Demo Applications

This folder contains end-to-end example applications demonstrating NDP-EP capabilities.

## Contents

| Example | Description |
|---------|-------------|
| [GOES-18 Utah Satellite](./goes-utah-satellite.ipynb) | Process GOES-18 satellite imagery for Utah region with interactive Folium visualization |

## Running Examples

### Google Colab

Most notebooks include an "Open in Colab" badge at the top. Click it to run the notebook directly in Google Colab without local setup.

### Local Execution

1. Create a virtual environment:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

2. Install dependencies:
   ```bash
   pip install ndp-ep xarray netCDF4 pyproj folium numpy requests pillow
   ```

3. Open Jupyter:
   ```bash
   pip install jupyter
   jupyter notebook
   ```

## Prerequisites

- Access to an NDP-EP API instance
- Valid authentication token
- See each example for specific requirements
