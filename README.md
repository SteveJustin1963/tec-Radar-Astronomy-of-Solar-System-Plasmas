# tec-Radar-Astronomy-of-Solar-System-Plasmas RASP

Catch the whispers of the Sun! Use simple radio gear to detect how the solar wind shapes Earth's ionosphere and space weather in real time



# **DIY Lab: Detecting the Solar Wind with TEC-1 (Z80 SBC) & VLF Receiver**

## **Objective**
Detect and analyze variations in Earth's ionosphere caused by the solar wind using a **Very Low Frequency (VLF) receiver** and process the data with a **TEC-1 Z80 SBC**.

## **Background**
The **solar wind**—a stream of charged particles from the Sun—affects Earth's **ionosphere**, altering how radio waves propagate. By monitoring a **VLF transmitter signal**, we can observe changes in signal strength and phase shifts that correlate with solar wind activity.

## **Materials Needed**
1. **TEC-1 SBC (Z80-based computer)** – for data logging and analysis.
2. **VLF Receiver** (homemade or kit-based) – to capture signals from stable VLF transmitters.
   - Example: **WWVB (60 kHz), NAA (24 kHz), or DHO38 (23.4 kHz)**
3. **Ferrite Rod or Loop Antenna** – to pick up VLF signals.
4. **Audio Amplifier Module** – for signal amplification.
5. **Analog-to-Digital Converter (ADC)** (e.g., **ADC0804**) – to digitize the VLF signal for the TEC-1.
6. **Basic RF filters** – for noise reduction.
7. **Power Supply** – 5V & 12V for the circuits.
8. **Oscilloscope or SDR (Optional)** – to visualize signals before sending them to the Z80.

---

## **Step 1: Building the VLF Receiver**
1. **Antenna Setup**: 
   - Wind **100-200 turns** of **magnet wire** around a ferrite rod or make a **loop antenna (1m diameter)**.
   - Connect to a high-impedance preamplifier.
   
2. **Tuning Circuit**:
   - Use a **LC resonant circuit** tuned to ~20-60 kHz.
   - Adjust capacitor values to maximize signal pickup from a chosen VLF station.

3. **Amplification**:
   - Use an **op-amp** (e.g., **TL072**) to boost the weak VLF signal.

4. **Demodulation**:
   - The signal is typically AM-modulated. A simple **diode detector** can extract envelope changes.

5. **Filtering & ADC Input**:
   - Filter out **high-frequency noise** with an **RC low-pass filter**.
   - Feed the signal into an **ADC0804 (8-bit ADC)**, interfaced with the TEC-1 SBC.

---

## **Step 2: Connecting the VLF Receiver to the TEC-1**
1. **Wiring the ADC to the TEC-1**
   - **ADC0804 Pinout**:
     - **Vin+** → Filtered VLF signal output.
     - **Vin-** → Ground.
     - **CS, RD, WR, INTR** → Connected to TEC-1 **I/O ports (e.g., Port 3)**.
     - **DB0-DB7** → Z80 **data bus**.

2. **Z80 Assembly Code to Read ADC**
   - This code reads the ADC data, storing signal strength over time.

```assembly
; TEC-1 ADC Read Routine
IN ADC_PORT       ; Read ADC value from port
MOV A, L          ; Store signal level
CALL STORE_DATA   ; Save for later analysis
JMP LOOP
```

3. **Logging and Displaying Data**
   - Display signal strength variations on the **TEC-1 7-segment display**.
   - Store data in memory for later analysis.

---

## **Step 3: Detecting Solar Wind Variations**
1. **Baseline Measurement**:
   - Record VLF signal levels over a 24-hour period.
   - Identify daily variations due to the **ionosphere's normal behavior** (e.g., night-day transitions).

2. **Solar Wind Effects**:
   - **Solar Flares or CMEs (Coronal Mass Ejections)** cause **sudden changes** in the VLF signal.
   - **Increased signal absorption** during solar events = **lower signal strength**.
   - Compare data logs to **solar activity reports** from sources like NOAA’s Space Weather Prediction Center.

---

## **Step 4: Analyzing Results**
- **Plot** signal variations vs. time using:
  - **TEC-1's display** (for simple monitoring).
  - **Dumping data to a PC via serial interface** for graphing.

- Compare with **solar wind speed/density data** from online sources.

---

## **Bonus: Expanding the Project**
- Use **two VLF receivers** at different locations to compare results.
- **Use an RTC (Real-Time Clock) module** to timestamp data.
- Build a **Z80-based data logger** for long-term monitoring.

---

## **Conclusion**
This DIY project enables real-time monitoring of **solar wind effects on Earth's ionosphere** using simple radio equipment and the **TEC-1 SBC**. By capturing VLF signal variations, we can **detect space weather events** and gain insights into solar activity—**all from home!** 


# Mathematical Formulas
The Radar Astronomy of Solar System Plasmas 19650001544.PDF  contains relevant mathematical formulas that can be applied to your DIY **solar wind detection** project using the **TEC-1 SBC and a VLF receiver**. Here’s how the key equations from the paper relate to your setup:

---

### **1. Doppler Excess Frequency & Electron Density Changes**
- The document provides this equation for detecting changes in electron density using radio waves:

![image](https://github.com/user-attachments/assets/becc33e8-c2bc-4192-b1dd-ede236a688f0)

\[
f_{DE} = \frac{c^2 r_e}{2} \frac{dI}{dt}
\]

where:
- \( f_{DE} \) = Doppler excess frequency (Hz)
- \( c \) = speed of light (~\( 3 \times 10^8 \) m/s)
- \( r_e \) = classical electron radius (~\( 2.8178 \times 10^{-15} \) m)
- \( I \) = columnar electron density (electrons/m²)
- \( \frac{dI}{dt} \) = rate of change of electron density

🔹 **How to use it in your DIY lab**:
- Your VLF signal undergoes **phase shifts** when solar wind alters the ionosphere’s electron density.
- The **TEC-1 SBC can measure** frequency variations in your VLF signal over time.
- **Comparing recorded \( f_{DE} \) changes with space weather data** can reveal **solar wind disturbances**.

---

### **2. Signal Path Delay Due to Electron Density**
The paper states that a wave traveling through a plasma slows down according to:

![image](https://github.com/user-attachments/assets/3ca168d7-7d99-46dd-8fa5-f100e4cf9a0c)

\[
R = \frac{c^2 r_e}{2} I
\]

where:
- \( R \) = additional path delay (meters)
- \( I \) = integrated electron density (electrons/m²)

🔹 **Application to Your Setup**:
- If the **VLF signal takes longer to arrive** at your receiver, it suggests an **increase in electron density** in the ionosphere due to solar wind.
- **TEC-1 can log timestamps** of signal changes and compute **ionospheric fluctuations**.

---

### **3. Refraction & Bending of Radio Waves**
For transverse electron density gradients, the wave path bends:

![image](https://github.com/user-attachments/assets/e1d9acad-ffb6-499a-adb0-0e924d7a0167)

\[
\Delta \theta = \frac{c^2 r_e}{2 f^2} \frac{dI}{dx}
\]

where:
- \( \Delta \theta \) = change in wave direction (radians)
- \( f \) = VLF signal frequency (Hz)
- \( \frac{dI}{dx} \) = electron density gradient

🔹 **How This Applies**:
- Sudden **signal fades or fluctuations** in your VLF reception could indicate **ionospheric bending**.
- By **tracking signal strength variations with TEC-1**, you might detect solar wind-driven ionospheric turbulence.

---

### **4. Faraday Rotation (Polarization Change)**
If you add a **polarized antenna**, you could measure **Faraday rotation**:

![image](https://github.com/user-attachments/assets/ab356ad2-b71f-4393-87f9-807cf7a26cb4)

\[
\phi = \frac{2 c r_e}{f^2} \int_{path} N_e B \, ds
\]

where:
- \( \phi \) = rotation angle (radians)
- \( B \) = Earth’s magnetic field strength
- \( N_e \) = electron density

🔹 **Why This Matters**:
- This could be an **advanced experiment** where you measure **VLF signal polarization shifts** to infer **magnetic field interactions with solar wind**.
- **DIY challenge**: Use a **circularly polarized loop antenna** and detect **small changes in signal polarization** with TEC-1.

---

## **Conclusion**
The **mathematics in the PDF** aligns well with our **DIY TEC-1 + VLF solar wind detection** project. The **Doppler shift, path delay, and signal bending** formulas can be used to **interpret real-time VLF data** logged by your Z80 system. 
We could **derive simpler versions of these equations** for direct implementation in Z80 assembly.
