![image](https://github.com/user-attachments/assets/c7d25423-398a-4653-831b-e581cd0fa629)# tec-Radar-Astronomy-of-Solar-System-Plasmas RASP

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
We need the MPU AM9511 or we could **derive simpler versions of these equations** for direct implementation in Z80 assembly.


# **Point 1 (Doppler Excess Frequency & Electron Density Changes)**, 
we need to measure **changes in VLF signal frequency over time** to compute **electron density variations** caused by the solar wind. 

Here's what to measure and how to apply it to the formula:

---

### **1. What You Need to Measure**
1. ![image](https://github.com/user-attachments/assets/ab735f98-1f4d-457e-8f91-90356abf16cd) **\( f_{VLF} \)** – **VLF carrier frequency** (e.g., 24 kHz from NAA).
2. ![image](https://github.com/user-attachments/assets/87cc68b8-1b5b-4759-a345-d9416fb3b70e) **\( \Delta f \)** – **Frequency drift (Doppler shift) of the received VLF signal** in Hz.
3. ![image](https://github.com/user-attachments/assets/96314b4a-d9e1-466e-91db-bb492a89b545) **\( \frac{dI}{dt} \)** – **Rate of change of column electron density (to be computed).**
4. **\( c \) (Speed of Light)**  `c`    – Constant \( 3 \times 10^8 \) m/s. ![image](https://github.com/user-attachments/assets/04a75ea3-3a68-433b-b9c4-08b117408f55)

5. ![image](https://github.com/user-attachments/assets/a80887f9-615b-4e94-aafa-cb614f37efeb) **\( r_e \) (Classical Electron Radius)** – Constant \( 2.8178 \times 10^{-15} \) m.

---

### **2. Applying the Formula**
The PDF gives the equation:

![image](https://github.com/user-attachments/assets/bdb2cfe5-d9c4-4727-86cf-48c588705741)

\[
f_{DE} = \frac{c^2 r_e}{2} \frac{dI}{dt}
\]

Rearrange to solve for **\( dI/dt \) ![image](https://github.com/user-attachments/assets/11264e33-baa6-464e-9348-db87c0f004e3) (electron density rate of change):**

![image](https://github.com/user-attachments/assets/485e4a5e-6443-4801-9008-52a78fb8a680)

\[
\frac{dI}{dt} = \frac{2 f_{DE}}{c^2 r_e}
\]

where:
- \( f_{DE} \) is the measured **Doppler excess frequency** (Hz).
- \( dI/dt \) is the **rate of change of column electron density (electrons/m²/s).**

---

### **3. Steps to Measure & Calculate \( dI/dt \)** ![image](https://github.com/user-attachments/assets/31f086c5-3dd0-457f-890d-a55693e5f52f)

1. **Record the baseline VLF signal frequency \( f_{VLF} \).** ![image](https://github.com/user-attachments/assets/d0c5550f-1cf9-4eb1-b046-79190df88a73)

   - Example: **WWVB = 60 kHz**, **NAA = 24 kHz**, **DHO38 = 23.4 kHz**.
   - Use **TEC-1’s counter/timer circuit** to log frequency over time.

2. **Measure the frequency shift \( \Delta f \) ![image](https://github.com/user-attachments/assets/49154b83-ffe2-44ab-8e31-f325db3270aa)
 at different times.**
   - If the received VLF signal **drifts up/down**, this is the Doppler effect.
   - Example: If the expected signal is **24.000 kHz** but fluctuates to **24.002 kHz**, then **\( \Delta f = 2 \) Hz**. ![image](https://github.com/user-attachments/assets/afcd3fb0-a26a-4118-a373-3757396c689d)


3. **Compute \( f_{DE} \):** ![image](https://github.com/user-attachments/assets/40900ea9-6510-4d36-9bc2-ea5861a4d802)

   - \( f_{DE} = \Delta f \) ![image](https://github.com/user-attachments/assets/6054f692-042c-4a80-bfaa-16c47996c2eb)
(assuming minimal movement between receiver and transmitter).

4. **Plug into the equation:**
   - Example:
     - If \( f_{DE} = 2 \) Hz ![image](https://github.com/user-attachments/assets/7f51457a-9610-44c2-bf02-6bd9b828c635)

     - Then:
![image](https://github.com/user-attachments/assets/5d0ad066-e7d9-4aaf-9d3f-2af814e79339)

       \[
       \frac{dI}{dt} = \frac{2 \times 2}{(3 \times 10^8)^2 \times 2.8178 \times 10^{-15}}
       \]

     - Approximate result:
![image](https://github.com/user-attachments/assets/2e77bf09-b458-4a44-af30-2a50381b3289)

       \[
       dI/dt \approx 1.48 \times 10^{12} \text{ electrons/m²/s}
       \]

     - Meaning: The **column electron density** in the ionosphere is changing at a rate of 1.48 x 10^12 electrons per square meter per second due to solar wind interaction.

---

### **4. How TEC-1 Can Implement This**
- Use **Z80 timer interrupts** to count **frequency changes per second**.
- Store frequency readings in **TEC-1’s memory** at fixed time intervals.
- Compute **\( f_{DE} \) ![image](https://github.com/user-attachments/assets/2b8662c8-50b7-4496-9b97-ce2827b3bc24)
by comparing consecutive readings**.
- Calculate \( dI/dt \) ![image](https://github.com/user-attachments/assets/18f063ac-53ce-4014-9806-1e8d8c8579d5)
using the formula in **integer math** (scaled for 16-bit precision).
- Display the **solar wind intensity estimate** in **scientific notation** or **LED bar format**.

---

### **5. Expected Observations**
- **Stable signal** = Normal ionosphere.
- **Slow drift in \( f_{DE} \)** ![image](https://github.com/user-attachments/assets/9f79bbe3-1a32-4ced-815d-13ac43fd18ff)
= Natural ionospheric fluctuations.
- **Sudden jump in \( f_{DE} \) (> 5 Hz over minutes)** ![image](https://github.com/user-attachments/assets/83a3bbb9-2714-40c9-b2f0-4398123cc7d7)
= Possible **solar flare or CME (coronal mass ejection)** impact.
- **Prolonged \( f_{DE} \) shift over hours** ![image](https://github.com/user-attachments/assets/37bf0352-6404-454d-9925-ca615fcfc168)
= Strong solar wind stream.

---

## **Conclusion**
To detect solar wind, you **only need to measure**:
1. **VLF frequency \( f_{VLF} \) ![image](https://github.com/user-attachments/assets/06991ba6-fa7d-4b8e-839b-20eb8359c308)
over time.**
2. **Frequency drift \( f_{DE} \).** ![image](https://github.com/user-attachments/assets/2952aac6-1fe8-4cd1-9cbb-dc4adaa36c23)

3. **Apply the formula** to get **\( dI/dt \), ![image](https://github.com/user-attachments/assets/3c6312ad-8bae-48a6-8385-07cf5b52eee8)
the rate of ionospheric electron density change.**

 ////

# Arduino 
- with **32-bit floating-point math** and a **transcendental function library**,
- we can directly implement the full **solar wind detection system**
  - using a **VLF receiver,
  - a frequency counter (Timer1),
  - and an ADC** to measure Doppler shifts and ionospheric electron density variations.  

---

## **📌 Key Features of This Code**
✅ Uses **Arduino Timer1** (16-bit) to measure VLF frequency.  
✅ Uses **ADC (AnalogRead)** to measure VLF signal amplitude variations.  
✅ Computes **Doppler shift (\( f_{DE} \))**.  
✅ Computes **Electron Density Rate of Change (\( dI/dt \))** using **floating-point math**.  
✅ Outputs all results **via Serial Terminal**.  

---

## **🛠️ Hardware Setup**
1. **VLF Receiver Output (Analog Signal)** → Connect to **A0 (ADC input)**.
2. **VLF Frequency Output (Digital Signal)** → Connect to **Pin 5 (Timer1 Capture Input)**.
3. **Serial Monitor (115200 baud)** → Displays real-time computed data.

---

## **📜 Full Arduino Code**
```cpp
// Constants
const float c = 3.0e8;          // Speed of light in m/s
const float r_e = 2.8178e-15;   // Classical electron radius in meters
const float K = 2 / (c * c * r_e); // Precomputed scaling factor for dI/dt

// Global Variables
volatile unsigned long prevTime = 0; // Previous frequency measurement timestamp
volatile unsigned long currTime = 0; // Current frequency measurement timestamp
volatile float F_old = 24000.0;      // Previous VLF frequency (start with 24 kHz)
float F_new = 0.0;                   // Current measured frequency
float f_DE = 0.0;                    // Doppler excess frequency
float dI_dt = 0.0;                    // Electron density rate of change
int signalStrength = 0;               // VLF amplitude measurement

void setup() {
    Serial.begin(115200); // Initialize Serial Monitor
    pinMode(5, INPUT);    // Timer1 input pin for frequency counting
    pinMode(A0, INPUT);   // ADC input for VLF amplitude

    // Setup Timer1 for Frequency Measurement
    TCCR1A = 0;           // Normal mode
    TCCR1B = (1 << CS11) | (1 << CS10); // Prescaler = 64
    TIMSK1 |= (1 << TOIE1); // Enable Timer Overflow Interrupt

    Serial.println("Solar Wind Detector Initialized...");
}

void loop() {
    // Measure Frequency using Timer1
    noInterrupts();          // Disable interrupts temporarily
    currTime = micros();     // Capture current time in microseconds
    unsigned long timeDiff = currTime - prevTime;
    prevTime = currTime;     // Update previous time
    interrupts();            // Enable interrupts again

    if (timeDiff > 0) {
        F_new = 1e6 / timeDiff;  // Convert time period to frequency in Hz
    }

    // Measure Signal Amplitude using ADC
    signalStrength = analogRead(A0);

    // Compute Doppler Shift (f_DE)
    f_DE = abs(F_new - F_old);

    // Compute Electron Density Rate of Change (dI/dt)
    dI_dt = f_DE * K;

    // Print Results to Serial Monitor
    Serial.print("F_new: ");
    Serial.print(F_new, 2);
    Serial.print(" Hz | f_DE: ");
    Serial.print(f_DE, 5);
    Serial.print(" Hz | dI/dt: ");
    Serial.print(dI_dt, 5);
    Serial.print(" e-/m²/s | Signal Strength: ");
    Serial.println(signalStrength);

    // Update Old Frequency for Next Calculation
    F_old = F_new;

    // Delay before next measurement (Adjust based on desired update rate)
    delay(500);
}

// Timer1 Overflow Interrupt for Frequency Measurement
ISR(TIMER1_OVF_vect) {
    prevTime = micros(); // Capture timestamp when overflow occurs
}
```

---

## **🔎 Explanation of Code**
### **1️⃣ Frequency Measurement Using Timer1**
- Uses **Timer1** on **Pin 5** to count pulses from the **VLF receiver**.
- Computes **time between pulses** and converts to frequency (Hz).

### **2️⃣ Amplitude Measurement Using ADC**
- Uses **analogRead(A0)** to measure **VLF signal amplitude**, which fluctuates due to **ionospheric conditions**.

### **3️⃣ Compute Doppler Shift (\( f_{DE} \))**
- **Formula:** \( f_{DE} = |F_{new} - F_{old}| \)
- This gives the **rate of frequency shift**, which is directly related to **solar wind disturbances**.

### **4️⃣ Compute Electron Density Rate of Change (\( dI/dt \))**
- **Formula:**
  \[
  dI/dt = f_{DE} \times \frac{2}{c^2 r_e}
  \]
- Uses **precomputed constant \( K \)** for efficiency.

### **5️⃣ Display Data on Serial Terminal**
- Prints **Frequency, Doppler Shift, Electron Density Rate, and Signal Strength** to the **Arduino Serial Monitor (115200 baud)**.

---

## **📊 Example Output on Serial Monitor**
```
Solar Wind Detector Initialized...
F_new: 24001.24 Hz | f_DE: 1.24 Hz | dI/dt: 3.00e+12 e-/m²/s | Signal Strength: 512
F_new: 23999.87 Hz | f_DE: 1.37 Hz | dI/dt: 3.32e+12 e-/m²/s | Signal Strength: 518
F_new: 24002.45 Hz | f_DE: 2.58 Hz | dI/dt: 6.25e+12 e-/m²/s | Signal Strength: 510
```
- **F_new** → Current measured **VLF frequency**.
- **f_DE** → Detected **Doppler shift**, indicates **ionospheric changes**.
- **dI/dt** → Computed **electron density rate**, linked to **solar wind events**.
- **Signal Strength** → Monitors **wave amplitude changes**, useful for **ionospheric disturbances**.

---

## **🚀 Enhancements & Next Steps**
✅ **Add a graphical display (OLED/LCD) for real-time visualization.**  
✅ **Log data to an SD card for long-term monitoring.**  
✅ **Trigger alerts if solar activity exceeds a threshold.**  
✅ **Integrate with space weather API for real-time comparisons.**  

# cross code this for matlab and see what we get




    
  
