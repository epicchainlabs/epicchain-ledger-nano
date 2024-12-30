---
# EpicChain Ledger Application

Welcome to the **EpicChain Ledger Application** for Nano S/X devices. This application serves as a bridge to manage your EpicChain transactions securely, leveraging the power of Ledger's trusted hardware wallet technology. With EpicChain, you gain access to an ecosystem designed for innovation, security, and efficiency, ensuring your digital assets remain protected while offering unparalleled usability.

---
## **Prerequisites**

Before diving into the setup process, it is crucial to ensure your development environment is properly configured. Follow Ledger's comprehensive [Getting Started Guide](https://ledger.readthedocs.io/en/latest/userspace/introduction.html) to get acquainted with the necessary steps. However, it’s worth noting that the official documentation, as of this writing, might feel convoluted, outdated, or occasionally incomplete. To address these issues, we’ve created an updated and streamlined guide: [Setup Environment](./SETUP_ENVIRONMENT.md). This guide simplifies the process and provides detailed solutions to common challenges.

### **Setting Up Environment Variables**

To establish a seamless connection with the Ledger development environment, you need to set specific environment variables:

1. **BOLOS_ENV Variable:** Point to the development environment path:
   ```bash
   BOLOS_ENV=/opt/bolos-devenv
   ```

2. **BOLOS_SDK Variable:** Define the secure SDK path:
   ```bash
   BOLOS_SDK=/opt/nanos-secure-sdk
   ```

By configuring these variables, you ensure smooth interactions with the development tools and SDKs essential for building and deploying applications on Ledger devices.

---
## **Compilation**

Compiling the EpicChain application is a straightforward process, thanks to the provided makefiles and tools. The following commands will help you prepare and load the application onto your Ledger device:

1. **Compile the Application:**
   ```bash
   make DEBUG=1  # Compile with optional PRINTF for debugging purposes
   ```

2. **Load the Application:**
   ```bash
   make load  # Load the compiled application onto the Nano S/X using ledgerblue
   ```

These steps ensure that your application is ready for deployment and debugging, providing a seamless development experience.

---
## **Documentation**

Understanding the inner workings of the EpicChain application is made easier with our comprehensive developer documentation. This includes detailed insights into key functionalities, ensuring that developers can leverage the full potential of the application.

### **Key Documentation Features**

- **[APDU Protocol](doc/APDU.md):** Gain a detailed understanding of the Application Protocol Data Units (APDU), which form the core of Ledger device communication.
- **[Commands](doc/COMMANDS.md):** Explore the supported commands and their usage in various scenarios.
- **[Transaction Serialization](doc/TRANSACTION.md):** Learn about transaction data handling and serialization processes.

### **Generating Documentation**

To generate high-quality developer documentation, use [Doxygen](https://www.doxygen.nl), a powerful tool for creating professional-grade documentation. Run the following command:

```bash
doxygen .doxygen/Doxyfile
```

This command produces documentation in two formats:
- **HTML Documentation:** Available in the `doc/html` folder for online viewing.
- **LaTeX Documentation:** Stored in the `doc/latex` folder for printable formats.

By providing both online and printable formats, we cater to diverse developer preferences, ensuring accessibility and usability.

---
## **Testing & Continuous Integration**

To maintain the highest code quality and reliability, our development process incorporates a robust Continuous Integration (CI) pipeline powered by [GitHub Actions](https://github.com/features/actions). This pipeline includes several automated steps to validate and optimize the application:

### **CI Workflow Steps**

1. **Code Formatting:**
   - Enforce consistent code standards using [clang-format](http://clang.llvm.org/docs/ClangFormat.html).

2. **Application Compilation:**
   - Use [ledger-app-builder](https://github.com/LedgerHQ/ledger-app-builder) to compile the application efficiently for Nano S/X devices.

3. **Unit Testing:**
   - Validate C functions with [cmocka](https://cmocka.org/) to ensure correctness and reliability.

4. **End-to-End Testing:**
   - Simulate device interactions using the [Speculos Emulator](https://github.com/LedgerHQ/speculos), verifying the application's functionality from start to finish.

5. **Code Coverage Analysis:**
   - Assess and report coverage with [gcov](https://gcc.gnu.org/onlinedocs/gcc/Gcov.html) and [lcov](http://ltp.sourceforge.net/coverage/lcov.php). Upload the results to [codecov.io](https://about.codecov.io) for detailed analysis.

6. **Documentation Generation:**
   - Automatically generate up-to-date documentation with [Doxygen](https://www.doxygen.nl).

### **Artifacts Produced**

The CI pipeline generates the following artifacts to aid development and debugging:

- **Debug Build Artifacts:** Includes the `boilerplate-app-debug`, providing output files from the compilation process in debug mode.
- **APDU Command Logs:** Stored in `speculos-log`, detailing APDU command/response interactions during end-to-end tests.
- **Code Coverage Reports:** Generated in `code-coverage` with HTML details of code coverage.
- **Documentation:** Auto-generated in `documentation`, providing HTML versions for developer reference.

By integrating these processes, we ensure a reliable, well-documented, and high-performing application.

---
## **Acknowledgments**

We extend our heartfelt thanks to **[Edouard Merle](https://github.com/Saltari)** from Ledger for his invaluable support throughout the development process. His prompt responses to queries, follow-ups on bug reports, and assistance in troubleshooting critical issues have been instrumental in the success of this project. Your dedication and expertise are deeply appreciated. ❤️

---

