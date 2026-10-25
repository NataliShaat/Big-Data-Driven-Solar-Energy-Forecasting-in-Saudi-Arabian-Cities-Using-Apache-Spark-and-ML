# Data

`KSAsolar2024.csv` is redistributed unchanged under its original **CC BY 4.0** licence.

- **Source:** S. Liaquat, *2024 Solar Dataset of Top 5 Cities of KSA*, Kaggle, 2024.
- **URL:** https://www.kaggle.com/datasets/shahlaliaquat/2024-solar-dataset-of-top-5-cities-of-ksa
- **Contents:** 10,980 rows × 13 columns: date, city, latitude, longitude, temperature (°C), weather, panel type, mount type, tilt (°), panel efficiency (%), solar irradiance (kWh/m²/day), output power (W), and estimated daily output (kWh, the target).
- **Encoding:** Latin-1 (read with `encoding='latin1'`).

The data are **simulated**. See the data audit in the notebook: the target is constant per City × Month × Mount and identical across panel types.
