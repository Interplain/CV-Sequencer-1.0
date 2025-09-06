# Arduino CV/Gate Sequencer

A professional 8-step CV/Gate sequencer with musical intelligence, pattern variations, and real-time control for modular synthesizers and eurorack systems.

![Project Status](https://img.shields.io/badge/status-active-brightgreen)
![Platform](https://img.shields.io/badge/platform-Arduino-blue)
![License](https://img.shields.io/badge/license-MIT-green)

## 🎵 Features

- **8-Step Sequencer** - Individual CV and gate control per step
- **Variable Step Lengths** - 4, 8, 12, 16, or 32 steps per sequence
- **16 Built-in Patterns** - Pre-programmed musical patterns for each step length
- **Musical Intelligence**:
  - Chromatic scale support
  - Tonic & Dominant mode
  - Tonic, Subdominant & Dominant mode
- **Real-time CV Control** - External CV inputs for pattern and root note modulation
- **Step Frequency Divider** - 1, 2, 3, 4, 8, 16, 32, 64x clock division
- **Pattern Storage** - EEPROM memory for saving custom patterns
- **Live Step Editing** - Adjust CV levels and gate states in real-time
- **Clock Sync** - External clock input with 5ms gate outputs

## 📺 Display Interface

Based on the actual hardware implementation, here's what the OLED display looks like:

### **Main Sequencer Display** (128x64 OLED)

![20240917_002229](https://github.com/user-attachments/assets/3661208d-fceb-42c5-bdb1-61466dd68604)


### **Front Panel** (Gerber File)
<img width="273" height="681" alt="image" src="https://github.com/user-attachments/assets/6ab75ed5-93d9-4e28-a730-21b804ba233e" />


### **Main Board** (Gerber File)
<img width="273" height="693" alt="image" src="https://github.com/user-attachments/assets/8c3969b8-270c-4357-9271-d68584eff160" />


### **Actual Display Photos Analysis**

**Photo 1 shows:**
- Step progress bar at top
- 8 CV level bars with different heights
- Current playing step highlighted (filled white)
- Musical notes: `C D E F F F C D`
- `ROT:31 ST:8 PTN:3`
- `SCL:TD SW:1 /1 RS SV`

### **Display Elements**
- **Progress Bar** - Thin line at top showing sequence position
- **CV Bars** - Simple vertical rectangles, height = pitch level
- **Current Step** - Playing step filled solid white, others outlined
- **Musical Notes** - Single letter names (C, D, E, F, G, A, B)
- **Sharp/Flat Notes** - Displayed with inverted text (white on black)
- **Selection Triangle** - Points to currently selected step/parameter
- **Parameters** - Compact text layout:
  - **ROT** = Root note offset
  - **ST** = Step length (4/8/12/16/32)
  - **PTN** = Pattern number (1-16)
  - **SCL** = Scale mode (CRM/TD/TSD)
  - **SW** = Clock divider ratio
  - **RS** = Reset function
  - **SV** = Save function

## 🛠️ Hardware Requirements

### Core Components
- **Arduino Uno/Nano** or compatible
- **SSD1306 OLED Display** (128x64, SPI interface)
- **Rotary Encoder** with push button
- **MCP4725 DAC** (I2C, 12-bit) for CV output
- **External Clock Input** (digital)
- **CV Inputs** for real-time control

### Circuit Components
- **Pull-up resistors** for encoder (if needed)
- **Input protection** for clock/CV inputs
- **Output buffering** for gate signal (optional)

## 📋 Pin Configuration

```cpp
// Display (SPI)
MOSI:  Pin 9
CLK:   Pin 10  
DC:    Pin 11
CS:    Pin 12
RESET: Pin 13

// User Interface
Encoder A: Pin 2 (interrupt)
Encoder B: Pin 3 (interrupt) 
Button:    Pin 5 (INPUT_PULLUP)

// Sequencer I/O
Clock In:  Pin 6 (digital input)
Gate Out:  Pin 4 (digital output)
CV Out:    I2C DAC (MCP4725) - Address 0x60

// CV Inputs (analog)
Root CV:   A6 (0-5V for root note control)
Pattern CV: A3 (0-5V for pattern selection)
```

## 🚀 Installation

### Prerequisites
- **Arduino IDE** 1.8.x or newer
- Required libraries:
  - `Encoder` by Paul Stoffregen
  - `Adafruit SSD1306`
  - `Adafruit GFX Library`
  - `Wire` (built-in)
  - `EEPROM` (built-in)

### Setup Steps

1. **Install libraries** through Arduino Library Manager
2. **Wire hardware** according to pin configuration
3. **Upload code** to Arduino
4. **Calibrate CV inputs** (optional)
5. **Connect to modular system**

### Wiring Diagram

```
Arduino Connections:
├── Display (SSD1306 SPI)
│   ├── VCC → 5V
│   ├── GND → GND
│   ├── MOSI → Pin 9
│   ├── CLK → Pin 10
│   ├── DC → Pin 11
│   ├── CS → Pin 12
│   └── RESET → Pin 13
├── Rotary Encoder
│   ├── A → Pin 2 (interrupt)
│   ├── B → Pin 3 (interrupt)
│   └── Button → Pin 5 (pullup)
├── MCP4725 DAC (I2C)
│   ├── VCC → 5V
│   ├── GND → GND
│   ├── SDA → A4
│   └── SCL → A5
├── Sequencer I/O
│   ├── Clock In → Pin 6
│   ├── Gate Out → Pin 4
│   ├── Root CV → A6 (0-5V)
│   └── Pattern CV → A3 (0-5V)
└── Power Supply
    ├── +5V (for logic)
    └── GND (common)
```

## 🎹 Musical Features

### **Scale Modes**
- **CRM (Chromatic)** - All 12 semitones available
- **TD (Tonic & Dominant)** - Root and fifth only
- **TSD (Tonic, Subdominant & Dominant)** - Root, fourth, and fifth

### **Step Patterns** (16 patterns per length)
Each step length has 16 unique musical patterns:

#### **4-Step Patterns**
- Simple ascending/descending scales
- Arpeggiated sequences  
- Rhythmic variations

#### **8-Step Patterns**
- Extended melodic phrases
- Call and response patterns
- Complex rhythmic subdivisions

#### **12-Step Patterns**
- Compound meter sequences
- Extended harmonic progressions

#### **16-Step Patterns**
- Full phrase structures
- Advanced rhythmic patterns

#### **32-Step Patterns**
- Extended compositions
- Complex polyrhythmic structures

### **CV Quantization**
- **61-step CV table** for precise 1V/octave scaling
- **Musical note display** with proper sharp/flat notation
- **Real-time transposition** via Root CV input

## 🎛️ Operation Guide

### **Navigation**
- **Rotate encoder** to move between parameters/steps
- **Short press** to enter edit mode
- **Long press** (4+ seconds) to toggle gate on selected step

### **Parameter Overview**

| Parameter | Function | Range | Control |
|-----------|----------|-------|---------|
| **ROT** | Root note/transposition | 0-30 | Encoder + Root CV |
| **ST** | Step length | 4/8/12/16/32 | Encoder |
| **PTN** | Pattern selection | 1-16 | Encoder + Pattern CV |
| **SCL** | Scale mode | CRM/TD/TSD | Encoder |
| **SW** | Clock divider | 1,2,3,4,8,16,32,64 | Encoder |
| **RS** | Reset sequence | - | Press to reset |
| **SV** | Save pattern | - | Press to save |

### **Step Editing**
1. Select step 1-8 from main menu
2. **Rotate encoder** to adjust CV level (pitch)
3. **Long press** to toggle gate on/off
4. **Short press** to return to main menu

### **Pattern Management**
- **Load Pattern**: Select PTN parameter, rotate to choose
- **Save Pattern**: Edit steps, then select SV and press
- **External Control**: Connect CV to Pattern input for real-time switching

### **Performance Features**
- **Live Transposition**: Root CV input for real-time key changes
- **Pattern Morphing**: Pattern CV for smooth pattern transitions
- **Clock Division**: SW parameter for polyrhythmic effects
- **Gate Programming**: Individual gate control per step

## ⚙️ Technical Specifications

- **CV Output Range**: 0-5V (1V/octave standard)
- **CV Resolution**: 12-bit (4096 steps)
- **Gate Output**: 5V, 5ms pulses
- **Clock Input**: 5V digital, rising edge triggered
- **Update Rate**: Real-time, interrupt-driven
- **Memory**: 16 patterns × 5 step lengths stored in EEPROM
- **Latency**: < 1ms gate/CV response time

## 📁 Code Structure (Recommended Refactoring)

The current code is monolithic and should be split into modules:

### **Suggested File Structure**
```
CVSequencer/
├── CVSequencer.ino           // Main sketch
├── Display.h/.cpp            // OLED display management
├── Encoder.h/.cpp            // Rotary encoder handling
├── Sequencer.h/.cpp          // Core sequencing logic
├── Patterns.h/.cpp           // Pattern data and management
├── Storage.h/.cpp            // EEPROM save/load functions
├── CVOutput.h/.cpp           // DAC and CV generation
├── MusicTheory.h/.cpp        // Scale and quantization
└── Config.h                  // Pin definitions and constants
```

### **Key Refactoring Benefits**
- **Modularity** - Easier to maintain and debug
- **Reusability** - Components can be used in other projects
- **Readability** - Clear separation of concerns
- **Testing** - Individual modules can be tested separately
- **Collaboration** - Multiple developers can work on different modules

## 🔧 Troubleshooting

### **Display Issues**
- Check SPI wiring connections
- Verify display initialization in setup()
- Test with simple "Hello World" sketch

### **CV Output Problems**
- Confirm MCP4725 I2C address (0x60)
- Check DAC power supply (5V)
- Verify quantization table accuracy
- Test with multimeter for proper voltage levels

### **Encoder Problems**
- Check interrupt pin connections (pins 2 & 3)
- Verify pullup resistors or INPUT_PULLUP
- Test encoder with simple counter sketch

### **Clock Sync Issues**
- Verify clock input signal levels (0-5V)
- Check for proper edge triggering
- Test with known good clock source

### **Memory/Storage Issues**
- EEPROM has limited write cycles (~100,000)
- Implement wear leveling for frequent saves
- Add data validation for corrupted patterns

## 🎯 Advanced Features & Modifications

### **Possible Enhancements**
- **Multiple CV Outputs** - Add more DACs for polyphonic output
- **MIDI Integration** - Add MIDI input/output for DAW sync
- **Swing/Humanization** - Add timing variations
- **Pattern Chaining** - Link patterns for song mode
- **Probability Gates** - Add randomness to gate triggers
- **Step Velocity** - Variable gate lengths per step
- **External Gate Inputs** - Trigger individual steps
- **LCD Display** - Larger display for more information

### **Performance Optimizations**
- Use lookup tables for complex calculations
- Optimize display refresh rate
- Implement double buffering for smooth animation
- Use hardware timers for precise timing

## ⚠️ Safety & Usage Notes

### **Electrical Safety**
- Verify voltage levels before connecting to modular system
- Use proper grounding between all connected devices
- Protect inputs from overvoltage conditions

### **Modular System Integration**
- Standard 1V/octave CV output scaling
- Compatible with most Eurorack and other CV systems
- Gate output suitable for triggering envelopes and VCAs

### **EEPROM Lifespan**
- Limit automatic saves to preserve EEPROM life
- Manual save operation recommended for pattern storage
- Consider external storage for frequent pattern changes

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Code modularization (see suggested structure above)
- Additional musical scales and modes
- User interface enhancements
- Performance optimizations
- Documentation improvements

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Arduino community for excellent libraries
- Modular synthesizer community for inspiration
- Music theory resources for scale implementations
- Open source hardware/software communities

---

**📻 Connect your modular system and start sequencing!**
