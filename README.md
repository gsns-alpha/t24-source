# T24 L3 Source Code & Configurations

This directory contains the actual Temenos T24 Local Customization (L3) source code, TAFJ components, configuration, and compiled JAR artifacts as demonstrated in the meeting video.

---

## Directory Structure

```text
t24-source/
├── src/                                  # Pure T24 BASIC (.b) Source Routines
│   ├── TPS.BP/
│   │   ├── M.TEST.PGM.b                  # Standalone Program: prints "Hello World - Program"
│   │   ├── M.TEST.RTN.b                  # Callable Subroutine: prints "Hello World - Routine"
│   │   └── TPS.BP.component              # TAFJ Component Definition for TPS.BP
│   └── TTF.BP/
│       ├── V.ID.RTN.b                    # T24 Application ID Validation Routine (checks for "HOHO")
│       └── TTF.BP.component              # TAFJ Component Definition for TTF.BP
├── tafc_components/                      # TAFC Data Structure & Header definitions
│   ├── TPS.BP.h, TPS.BP.getDataStructureFields.b, ...
│   └── TTF.BP.h, TTF.BP.getDataStructureFields.b, ...
├── jars/                                 # Pre-compiled JAR Packages
│   ├── TTF_BP.jar                        # Active library loaded by JBoss EAP 7.4
│   └── TPS_BP.jar                        # TPS package jar
├── conf/
│   └── tafj.properties                   # Live TAFJ R24 runtime & compiler configuration
└── t24_source_code.tar.gz                # Original complete server backup archive
```

---

## Source Routines Summary

### 1. `M.TEST.PGM.b`
- **Package**: `TPS.BP`
- **Type**: `PROGRAM`
- **Purpose**: Diagnostic executable to verify TAFJ runtime and database connection.
- **Run Command**:
  ```bash
  tRun M.TEST.PGM
  # Output: Hello World - Program
  ```

### 2. `M.TEST.RTN.b`
- **Package**: `TPS.BP`
- **Type**: `SUBROUTINE`
- **Purpose**: Subroutine routine callable from T24 or other BASIC programs.
- **Output**: `Hello World - Routine`

### 3. `V.ID.RTN.b`
- **Package**: `TTF.BP`
- **Type**: `SUBROUTINE` (Validation Hook)
- **Purpose**: Validation routine attached to `PGM.FILE` in T24.
- **Logic**:
  ```basic
  IF COMI = "HOHO" THEN
      E = "In ID Routine."
  END
  ```
- **T24 BrowserWeb Demonstration**: When entering ID `HOHO` on the T24 screen, it triggers this routine and displays error `"In ID Routine."`.

---

## Deployment & JBoss Module Integration

- **TAFJ Compilation**: `tCompile src` generates Java bytecode into `/home/tafjr24/DevOps/target/classes/com/temenos/t24/`.
- **Packaging**: Packed into `/home/tafjr24/bnk/ttflib/TTF_BP.jar`.
- **JBoss EAP 7.4**: Symlinked at `/home/tafjr24/jboss-eap-7.4/modules/com/temenos/t24/main/ttflib/TTF_BP.jar`.
- **Restart Command**:
  ```bash
  /home/tafjr24/jboss-eap-7.4/bin/jboss-cli.sh --connect --controller=172.10.1.200:10055 command=":shutdown(restart=true)"
  ```
- **T24 BrowserWeb URL**: `http://172.10.1.200:8145/BrowserWeb/servlet/BrowserServlet` (User: `TTFUSER1` / Password: `123456`)
