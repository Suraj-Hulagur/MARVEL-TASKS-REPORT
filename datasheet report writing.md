### Report: MQ-135 Gas Sensor

The **MQ-135** is a widely used **SnO₂-based semiconductor air-quality sensor** designed to detect a variety of gases. It operates on the principle that the resistance of tin dioxide (SnO₂), a type of metal-oxide semiconductor, changes when exposed to different concentrations of gases in the air.  

---

### Working Principle
- The sensor consists of a **sensitive layer of SnO₂** deposited on a ceramic substrate, with a **heater coil** underneath.  
- When the heater raises the temperature, oxygen molecules adsorb on the SnO₂ surface, capturing free electrons and increasing resistance.  
- When a target gas (e.g., **NH₃, CO₂, alcohol vapors, benzene, smoke**) comes in contact, it reacts with the adsorbed oxygen, releasing electrons back into the conduction band.  
- This reduces the resistance (Rs) of the sensor, and the change in Rs is correlated to the **gas concentration**.  

---

### Electrical Characteristics
- Operating voltage: **5 V**  
- Heater consumption: ~800 mW  
- Output signal: Analog (voltage across load resistor RL)  
- Sensitivity: Detects ppm levels of various gases  
- Typical circuit: Sensor connected in series with **RL** (10 kΩ to 47 kΩ). The voltage across RL changes with gas concentration.  

---

### Calibration and Measurement
- Each sensor must be calibrated against a reference concentration.  
- The resistance Rs is normalized with respect to R₀, where R₀ is the sensor resistance in clean air or a known reference gas.  
- Calibration curves (from the datasheet) provide **log–log plots of Rs/R₀ vs ppm** for gases like CO₂, NH₃, alcohol, toluene, and smoke.  
- Ambient factors such as **temperature and humidity** can influence Rs, and compensation is necessary for accurate readings.  

---

### Theoretical Model
- The MQ-135’s multi-gas response follows a **power-law relationship**:  

  Rs/R₀ = A × (Concentration)^(-B)  

  where A and B are empirical constants derived from the datasheet curves.  
- This behavior is compatible with the **Freundlich adsorption model**, which describes gas adsorption on solid surfaces.  
- Understanding this model helps explain why the sensor response is **non-linear** and why empirical fitting is required in real applications.  

---

### Applications
- Indoor air-quality monitoring systems  
- Detection of harmful gases in industrial environments  
- Smart packaging and food spoilage detection  
- Environmental monitoring (e.g., CO₂ levels in classrooms, offices)  
- IoT devices for smart cities and agriculture  

---

### Limitations
- **Non-selective**: responds to multiple gases, so cross-sensitivity is common.  
- **Needs calibration** for reliable ppm estimation.  
- Affected by **humidity, temperature, and aging** of the sensor.  
- Cannot directly identify which gas is present, only that gas concentration has changed.  

---

### Conclusion
The **MQ-135 gas sensor** is a low-cost and practical device for detecting a wide range of gases. While its response is not selective and requires careful calibration, it provides valuable insights into **air quality trends** and **gas presence**. The non-linear power-law behavior explained by the **Freundlich adsorption model** makes it essential to use empirical calibration for real-world deployment. With proper compensation techniques, MQ-135 remains a popular choice for **IoT air-quality monitoring and safety systems**.  

---

### Sources
- [Olimex MQ135 Datasheet](https://www.olimex.com/Products/Components/Sensors/Gas/SNS-MQ135/resources/SNS-MQ135.pdf)  
- [Winsen MQ135 Manual](https://www.winsen-sensor.com/d/files/PDF/Semiconductor%20Gas%20Sensor/MQ135%20(Ver1.4)%20-%20Manual.pdf)  
