<img src="https://raw.githubusercontent.com/PriyeshGosai/pypsa-meets-earth-lab-2025/main/img/top-banner.png" alt="Top Banner" width="100%">

# PyPSA Meets Earth Lab 2025

Welcome to the PyPSA Meets Earth Lab 2025 course materials! This repository contains interactive Jupyter notebooks and course materials for learning energy system modelling with PyPSA.

## About

This course provides hands-on training and practical examples for energy system modelling using PyPSA (Python for Power System Analysis). The materials are designed to help you understand and work with modern power and energy systems.

**Instructor:** Priyesh Gosai
**Organization:** Innovate for Impact
**Email:** priyesh@innovateimpact.com

## Quick Start

### Option 1: View Online

Visit the course website at: https://priyeshgosai.github.io/pypsa-meets-earth-lab-2025/

### Option 2: Google Colab

You can run the notebooks directly in your browser using Google Colab (no installation required):
- Click the rocket icon at the top of any lesson page and select "Colab"

### Option 3: Local Installation

1. **Clone this repository:**
   ```bash
   git clone https://github.com/PriyeshGosai/pypsa-meets-earth-lab-2025.git
   cd pypsa-meets-earth-lab-2025
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch Jupyter Lab:**
   ```bash
   jupyter lab
   ```

## Building the Book

If you'd like to build the Jupyter Book locally:

1. **Install jupyter-book:**
   ```bash
   pip install "jupyter-book<2"
   ```

2. **Build the book:**
   ```bash
   jupyter-book clean . --all
   jupyter-book build .
   ```

3. **View the book:**
   Open `_build/html/index.html` in your browser

## Course Contents

- **Part 1:** Getting started with PyPSA
- **Part 2:** Developing a model using Excel
- **Part 3: Compare scenarios**

## Case Study Disclaimer

**Disclaimer**

This analysis employs a South African energy system case study developed exclusively for educational purposes. The model has been intentionally simplified and should not be construed as an accurate or comprehensive representation of the contemporary South African energy system. Consequently, this model is not suitable for application in substantive studies, policy analysis, or operational assessments pertaining to the South African power system.

**Data Sources**

The development of this model incorporates data and information from the following sources:

* DeSantis, D., James, B.D., Houchins, C., Saur, G. and Lyubovsky, M. (2021) 'Cost of long-distance energy transmission by different carriers', *IScience*, 24(12).

* Electric Power Research Institute (EPRI) (2024) 'Supply-Side Cost and Performance Data for Eskom Integrated Resource Planning: 2023–2024 Update', Technical Report 3002031032. Palo Alto, California: EPRI.

* Eskom Transmission Division (2020) *Transmission Development Plan 2020–2029*. Johannesburg, South Africa: Eskom Holdings SOC.

* Merven, B. (2024) 'ESRG Hourly Demand Model v2025 by Province', September. doi: 10.25375/uct.26942134.v5

* Eskom Holdings SOC Ltd (2025) *Eskom*. Available at: https://www.eskom.co.za (Accessed: 24 November 2025).

* Meridian Economics (n.d.) *PyPSA-RSA: An Open Optimisation Model of the South African Power System*. Available at: https://github.com/MeridianEconomics/pypsa-rsa


## Resources

- **PyPSA Documentation:** https://pypsa.org
- **PyPSA-Earth:** https://github.com/pypsa-meets-earth
- **Course Repository:** https://github.com/PriyeshGosai/pypsa-meets-earth-lab-2025

## License

This course material is open source and available for educational purposes. See [LICENSE](LICENSE) for details.

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Credits

This project is created using the excellent open source [Jupyter Book project](https://jupyterbook.org/).
