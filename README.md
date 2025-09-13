# Kernel Chat: AI-Powered Embedded Linux Development Assistant
#### Last Tested: 24.08.2025 ✅

**Kernel Chat** is an advanced **AI-powered CLI tool** forked from **Google's Gemini-CLI**, specifically enhanced for **embedded Linux developers** and **kernel engineers**. It combines the power of Google's Gemini AI with specialized tools for embedded systems development, serial console debugging, intelligent code generation, and comprehensive documentation analysis.


See the kernel chat-CLI in action below!



### 🎥 [Demo Video](https://youtu.be/2kZJUMfhygE)

[![Demo Video](https://img.youtube.com/vi/2kZJUMfhygE/maxresdefault.jpg)](https://youtu.be/2kZJUMfhygE)

---

## 🚀 **Key Features**

### 🔗 **Serial Console Integration**
- **Direct hardware connection** to embedded devices (BeagleBone Black, Raspberry Pi, custom SoCs)
- **Real-time serial communication** with automatic logging and buffering
- **Interactive command execution** with natural language interpretation
- **Smart command translation**: Ask "show me CPU usage" → automatically runs appropriate commands
- **Live log summarization** using AI for easy debugging

### 📚 **Advanced Documentation RAG (Retrieval-Augmented Generation)**
- **Ingest technical documentation**: PDFs, datasheets, kernel docs, markdown files
- **Intelligent cross-referencing**: Connects hardware manuals with kernel documentation
- **Expert-level synthesis**: Provides comprehensive answers spanning multiple technical domains
- **Context-aware responses**: Understands embedded Linux ecosystem relationships

### 🛠️ **Specialized Embedded Linux Tools**
- **`get_device_info`** - Comprehensive device identification (CPU, memory, peripherals, board type)
- **`get_driver_info`** - Kernel module and driver analysis with hardware mapping
- **`kernel_hotspots`** - CPU profiling and performance bottleneck identification  
- **`real_time_analysis`** - Real-time system analysis (latency, scheduling, determinism)

### 🧠 **AI-Enhanced Debugging & Code Generation**
Ask natural language questions and get expert-level responses with automatic code generation:
- *"Why is my GPIO switching slower than expected on BeagleBone Black?"*
- *"How do I debug device tree issues for my I2C sensor?"*
- *"What's causing high interrupt latency in my real-time application?"*
- *"Show me the power management configuration for AM335x"*

**Built-in Code Generation Capabilities from Gemini CLI:**
- **Driver code generation**: Create complete Linux kernel drivers from specifications
- **Device tree overlays**: Generate DT overlays for custom hardware configurations  
- **Kernel module templates**: Scaffold new kernel modules with proper structure
- **Configuration files**: Generate kernel configs, Makefiles, and build scripts

### 🎯 **BeagleBone Black Specialization**
Built-in expertise for **Texas Instruments AM335x** platform:
- Deep understanding of ARM Cortex-A8 architecture
- Power management and boot sequence knowledge
- GPIO, I2C, SPI, UART configuration expertise
- PRU (Programmable Real-time Unit) integration
- Cape ecosystem and device tree overlay management

---

## 🛠️ **Installation**

### Prerequisites
- **Node.js 18+** ([Download](https://nodejs.org/en/download))
- **Serial hardware** 
- **Google AI API Key** or Google account for authentication

### Quick Start
```bash
# Clone the repository
git clone https://github.com/your-username/kernel-chat.git
cd kernel-chat

# Install dependencies
npm install

#Build
npm run build

# Start Kernel Chat
npm start
```

### Authentication
1. **Google Account** (recommended for personal use):
   - Sign in when prompted for up to 60 requests/minute
   
2. **API Key** (for higher limits):
   ```bash
   export GEMINI_API_KEY="your_api_key_here"
   ```
   Get your key from [Google AI Studio](https://aistudio.google.com/apikey)

---

## 🎮 **Usage Examples**

### Serial Console Connection
```bash
# Connect to your embedded device
/serial connect /dev/ttyUSB0 115200
```

### Send commands naturally

#### **`~` Natural Language Queries**  
- **What it does:** Converts your plain English questions into appropriate Linux commands  
- **AI Processing:** Automatically generates commands, sends them to your device, and provides intelligent summaries  
- **Best for:** Quick diagnostics, system analysis, and getting insights without knowing exact commands  

Examples:
```bash
   ~ Check the dmesg and summarize
   ~ What processes are consuming the most memory?
   ~ Show me network interface status
```

#### **`>` Direct Shell Commands**  
- **What it does:** Sends commands directly to your connected device  
- **Raw Output:** Shows exact command output without AI interpretation  
- **Best for:** Specific commands, debugging, and when you need control  

Examples:

```bash
    >uptime
    >free -h
    >lsusb
    >journalctl -f
```

#### **`>>>` Analysis & Summaries**  
- **What it does:** Analyzes recent output captured from your device without sending new commands  
- **AI Analysis:** Reviews logs and data already collected, providing insights and explanations  
- **Best for:** Understanding what happened, pattern analysis, and getting context about previous output

#### **Combined Usage: `'>' command >>> analysis`**  

Examples:

```bash
   >dmesg >>> summarize for errors
   >ps aux >>> which processes are using too much memory?
   >lsusb >>> are all USB devices working properly?
   >cat /var/log/syslog >>> what happened in the last boot?
```

### Device Analysis (tool calling)
```bash
# Get comprehensive device information
> get device info
```
This tool will call the `get_device_info` tool which provides:

- **System Identity Overview:** High-level "at-a-glance" summary of your Linux system's hardware and software configuration  
- **Hardware Information:** Device model, CPU architecture, and platform details for compatibility assessment  
- **Software Stack Details:** Kernel version, Linux distribution, and user-space environment information  
- **Initial System Assessment:** Quick orientation tool for understanding what you're working with before deep debugging  

```bash
# Analyze real-time performance
> analyze real-time performance on this system
```

This tool will call the `real_time_analysis` tool which provides:

- **RT Kernel Analysis:** Checks real-time kernel configuration, preemption settings, and boot parameters  
- **Interrupt & Scheduling Analysis:** Examines IRQ handling, RT task priorities, and CPU affinity settings  
- **Performance Diagnostics:** Identifies latency sources, priority inversions, and timing issues  
- **System Configuration Review:** Analyzes CPU isolation, power management, and memory settings affecting RT performance  
- **Hardware & Network RT Features:** Reviews CPU features, network stack configuration


```bash
# Check for kernel performance bottlenecks and interrupt issues
> get me the kernel hotspots connected to serial
```

This  will call the `kernel_hotspots` tool which provides:

- **CPU Utilization Analysis:** Shows how busy the processor was during a 5-second monitoring window  
- **Time Distribution Breakdown:** Details how CPU time was split between user space, kernel space, interrupts, and deferred work  
- **Performance Profiling:** Identifies the exact kernel functions consuming the most CPU time (when perf is available)  
- **Interrupt Analysis:** Shows the most frequent hardware interrupts and their rates  
- **System Health Assessment:** Provides a "medical check-up" for your Linux kernel's performance  

**What This Helps You Debug:**

- **Performance Issues:** Identify if CPU overload is causing system problems
- **Driver Problems:** Spot kernel functions that might be stuck in loops or consuming excessive resources  
- **Hardware Malfunctions:** Detect interrupt storms or abnormal hardware behavior
- **System Bottlenecks:** Understand where your kernel is spending its processing time
- **Resource Contention:** See if specific subsystems (network, I2C, timers) are overactive


```bash
> Get driver info from the serial device
```
This tool calls the `get_driver_info` tool which provides:

**Key Information:**
- **Overview:** High-level summary of how many devices were inspected  
- **Device Count:** Total hardware devices detected by the kernel  
- **Binding Status:** How many devices have drivers loaded vs. unbound  
- **Health Check:** Overall assessment of your hardware driver situation  


### Documentation Q&A with RAG
```bash
# Ingest documentation
/rag add linux-6.14.11/Documentation/devicetree --tag kernel
/rag add am335x_manual.pdf --tag beaglebone

# Ask complex technical questions
/ask "How do I configure GPIO interrupts on AM335x with proper device tree bindings?" --tag kernel
/ask "What are the power domain considerations for GPIO performance on BeagleBone Black?" --tag <tag you created>
```

### Natural Language Debugging
```bash
# Real-world problem solving
> My GPIO switching is only 1MHz instead of 10MHz on BeagleBone Black, what could be wrong?

> I'm getting kernel panics during high CPU load with GPIO operations, help me debug this

> Design a real-time system for motor control with sub-100μs response time on BeagleBone Black
```

### Code Generation Examples (With support from Gemini CLI)
```bash
# Driver development
> Create a Linux kernel driver for an SPI-connected accelerometer sensor

> Generate a device tree overlay for BeagleBone Black with I2C sensor at address 0x48

> Write a GPIO interrupt handler for BeagleBone Black with proper error handling

# System configuration
> Generate a kernel config for real-time BeagleBone Black system with PRU support

> Create a Makefile for building a custom kernel module with cross-compilation

> Write initialization scripts for embedded Linux system startup
```
---

## 🎯 **Specialized Commands**

### Serial Console Commands
- `/serial connect <port> <baud>` - Connect to device
- `/serial send <command>` - Send command to device  
- `/serial prompt <natural_language>` - AI-interpreted command execution
- `/serial summarize [query]` - AI analysis of logs
- `/serial disconnect` - Close connection

### RAG Documentation Commands  
- `/rag add <path> [--tag TAG]` - Ingest documentation
- `/rag list` - Show ingested documents
- `/rag status` - Show store information
- `/rag clear` - Clear documentation store
- `/ask "question"` - Query documentation with AI

### Memory Management
- `/memory show` - Display AI's current context
- `/memory add <text>` - Add information to AI memory
- `/memory refresh` - Reload GEMINI.md files
---

## 🛡️ **Security Considerations**

- **Development environment only**: Not intended for production systems
- **Serial access**: Tool executes commands directly on connected devices
- **Network isolation**: Use in controlled, isolated networks
- **Audit trail**: All commands and responses are logged

---

## 🚀 **Roadmap**

### Near-term Enhancements
- [ ] **Multi-board support** (Raspberry Pi, NVIDIA Jetson, custom SoCs)
- [ ] **Automated test generation** from device tree analysis
- [ ] **Performance regression detection** 
- [ ] **Visual system topology mapping**

### Advanced Features
- [ ] **Real-time anomaly detection** (kernel panics, OOM, IRQ storms)
- [ ] **Enhanced automated driver generation** from datasheet analysis and hardware specs
- [ ] **Complete system code generation** (bootloaders, kernel configs, userspace apps)
- [ ] **Team collaboration features** with shared knowledge base
- [ ] **Support for other OSs** (Zephyr , QNX)
- [ ] **To support other models** (OpenAI, Anthropic, Local LLM's..)

---

⚠️ **Developer Tool Notice**: This is a  development tool designed for embedded engineers and kernel developers. While functional, it's under active development with new features being added. Use it with caution in development environments.

## 📧 **Contact & Support**

For questions, issues, or contributions related to this Kernel Chat project:

**Email:** kondaraviteja1@gmail.com  
**GitHub:** [@Ravi-Teja-konda](https://github.com/Ravi-Teja-konda)

---

## 🤝 **Contributing**

We welcome contributions from the community! Please feel free to submit a pull request.

---

## 📊 **Technical Specifications**

- **AI Model**: Google Gemini (1M+ token context)
- **Supported Platforms**: Linux, macOS, Windows (with WSL)
- **Target Hardware**: ARM Cortex-A8/A9, ARM64, x86_64 embedded systems
- **Serial Protocols**: UART, USB-to-Serial adapters
- **Documentation Formats**: PDF, Markdown, HTML, plain text

---

## ❤️ **Acknowledgments**

- **Google Gemini Team** for the foundational Gemini-CLI
- **BeagleBoard.org** for excellent hardware and documentation
- **Linux Kernel Community** for comprehensive documentation

---

## 📄 **License**

This project is licensed under the **Apache License 2.0** - see the [LICENSE](LICENSE) file for details.

---


## ⭐ Support the Project
If you find this project useful, consider starring it on GitHub to help others discover it!
