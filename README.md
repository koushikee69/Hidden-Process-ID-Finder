
# Hidden Process Detection from Memory Dump (Trojan Analysis)

This project provides a Python script for detecting hidden processes from a memory dump file of the Bundes Trojan (0zapftis) using output from two Volatility plugins: `pslist` and `psxview`. The goal is to compare the process lists generated by both plugins and identify any discrepancies, which could indicate hidden processes or rootkits that are not visible in the regular process listing.

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [License](#license)

## Project Overview

The Bundes Trojan (0zapftis) is known for using advanced evasion techniques to hide its processes from typical process lists. This tool helps to identify any hidden processes by comparing the output of Volatility's `pslist` and `psxview` plugins.

## Features

- Extracts PIDs (Process IDs) from the output of `pslist` and `psxview`.
- Compares the extracted PIDs and identifies hidden processes (PIDs that appear in `psxview` but not in `pslist`).
- Easy-to-use Python script to automate the comparison process.

## Prerequisites

Before using this tool, make sure you have the following:

1. **Volatility** - A memory forensics tool to analyze memory dumps. You can install it by following the official documentation [here](https://volatility3.readthedocs.io/).
2. **Python 3.x** - The script is written in Python, so make sure you have Python 3.x installed on your system.
3. **Memory Dump File** - A memory dump of the infected system (e.g., `0zapftis.vmem`) that was obtained from the Trojan sample.

## Usage

1. Extract the memory dump file using the `unrar` command:
    ```bash
    unrar x 0zapftis.vmem.rar
    ```

2. Use the Volatility framework to run the `pslist` and `psxview` plugins on the memory dump file to generate the process lists:
    ```bash
    volatility -f 0zapftis.vmem --profile=YourProfile pslist > pslist_output.txt
    volatility -f 0zapftis.vmem --profile=YourProfile psxview > psxview_output.txt
    ```

3. Save the output of both commands (`pslist_output.txt` and `psxview_output.txt`) in the same directory as the Python script.

4. Run the Python script to compare the two files and identify any hidden processes:
    ```bash
    python compare_processes.py pslist_output.txt psxview_output.txt
    ```

5. The script will print any PIDs found in `psxview` that are not present in `pslist`, indicating hidden processes.

## How It Works

The script works by reading the output files from the `pslist` and `psxview` plugins. 
It then extracts the process IDs (PIDs) from both files, compares them, and identifies any PIDs that appear in `psxview` but not in `pslist`. 
These PIDs are likely associated with hidden processes used by the Trojan.

