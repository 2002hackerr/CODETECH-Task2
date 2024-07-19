# TASK TWO: VULNERABILITY SCANNING TOOL

## Project Details

- **Name:** PATHLAVATH MUKESH
- **Company:** CODTECH IT SOLUTIONS
- **ID:** CT12CCH156
- **Domain:** CYBER SECURITY AND ETHICAL HACKING
- **Duration:** 20th May 2024 to 20th July 2024
- **Mentor:** SRAVANI GOUNI

### Overview

This script provides a menu-driven interface for running various vulnerability scanning tools. It checks for the presence of the required tools, assists with their installation if needed, and handles the output of the scans.

## Features

- **Tool Availability Check**: Checks if a vulnerability scanning tool is installed.
- **Installation Assistance**: Prompts for installation and provides commands or instructions.
- **Scan Execution**: Runs the selected scan after providing target information.
- **Output Handling**: Offers to save scan results to a file or display them on the screen.
- **Color-Coded Outputs**: Uses different colors for prompts, messages, and outputs for better visibility.

## Supported Tools

- Nmap
- OpenVAS
- Nessus
- Nikto
- OpenSCAP
- Lynis
- Metasploit
- OWASP ZAP

## Prerequisites

- Bash shell
- sudo privileges for installing and running certain tools

## Installation

Clone the repository and navigate to the directory:

```bash
git clone https://github.com/2002hackerr/CODETECH-Task2.git
cd CODETECH-Task2
```

Make the script executable:

```bash
chmod +x CODETECH-Task2.sh
```

## Usage

Run the script:

```bash
./CODETECH-Task2.sh
```

Follow the on-screen prompts to select a vulnerability scanning tool, provide target information, and handle the scan output.

## Color Codes

- **Red**: Warnings and installation prompts
- **Green**: Success messages and file saves
- **Yellow**: Running scan messages
- **Blue**: Scan outputs displayed on screen
- **Cyan**: Information messages (e.g., starting services)

## Script Content

```bash
#!/bin/bash

echo "Created By = PATHLAVATH MUKESH "
echo "ID:CT12CCH156"

# Function to check if a command exists
command_exists() {
    command -v "$1" &> /dev/null
}

# Function to install a package
install_package() {
    read -p "The package $1 is not installed. Would you like to install it now? (y/n): " choice
    if [[ "$choice" == "y" || "$choice" == "Y" ]]; then
        if command_exists apt-get; then
            sudo apt-get update
            sudo apt-get install -y "$1"
        elif command_exists yum; then
            sudo yum install -y "$1"
        else
            echo -e "\e[31mUnsupported package manager. Please install $1 manually.\e[0m"
        fi
    fi
}

# Function to handle output
handle_output() {
    local output=$1
    read -p "Do you want to store the output in a file? (y/n): " choice
    if [[ "$choice" == "y" || "$choice" == "Y" ]]; then
        read -p "Enter the file name to save the output: " filename
        echo "$output" > "$filename"
        echo -e "\e[32mOutput saved to $filename.\e[0m"
    else
        echo -e "\e[34m$output\e[0m"
    fi
}

# Function to run Nmap scan
run_nmap() {
    read -p "Enter target for Nmap: " target
    output=$(nmap -A "$target")
    handle_output "$output"
}

# Function to run OpenVAS
run_openvas() {
    echo -e "\e[36mStarting OpenVAS...\e[0m"
    sudo openvas-start
    echo -e "\e[36mOpenVAS services started. Please use the OpenVAS web interface to run the scan.\e[0m"
}

# Function to run Nessus
run_nessus() {
    echo -e "\e[36mStarting Nessus...\e[0m"
    sudo /bin/systemctl start nessusd.service
    echo -e "\e[36mNessus services started. Please use the Nessus web interface to run the scan.\e[0m"
}

# Function to run Nikto scan
run_nikto() {
    read -p "Enter target for Nikto: " target
    output=$(nikto -h "$target")
    handle_output "$output"
}

# Function to run OpenSCAP
run_openscap() {
    read -p "Enter SCAP file for OpenSCAP: " scap_file
    read -p "Enter profile for OpenSCAP: " profile
    output=$(oscap xccdf eval --profile "$profile" "$scap_file")
    handle_output "$output"
}

# Function to run Lynis audit
run_lynis() {
    output=$(sudo lynis audit system)
    handle_output "$output"
}

# Function to run Metasploit
run_metasploit() {
    echo -e "\e[36mStarting Metasploit console...\e[0m"
    msfconsole
}

# Function to run OWASP ZAP
run_owasp_zap() {
    echo -e "\e[36mStarting OWASP ZAP...\e[0m"
    ./zap.sh
}

# Main menu function
main_menu() {
    PS3="Select a vulnerability scanning tool: "
    options=("Nmap" "OpenVAS" "Nessus" "Nikto" "OpenSCAP" "Lynis" "Metasploit" "OWASP ZAP" "Quit")
    select opt in "${options[@]}"; do
        case $opt in
            "Nmap")
                if command_exists nmap; then
                    echo -e "\e[33mRunning Nmap...\e[0m"
                    run_nmap
                else
                    install_package nmap
                    if command_exists nmap; then
                        echo -e "\e[33mRunning Nmap...\e[0m"
                        run_nmap
                    fi
                fi
                ;;
            "OpenVAS")
                if command_exists openvas-start; then
                    echo -e "\e[33mRunning OpenVAS...\e[0m"
                    run_openvas
                else
                    echo -e "\e[31mOpenVAS is not installed. Please follow the installation instructions at https://www.openvas.org/\e[0m"
                fi
                ;;
            "Nessus")
                if command_exists nessusd; then
                    echo -e "\e[33mRunning Nessus...\e[0m"
                    run_nessus
                else
                    echo -e "\e[31mNessus is not installed. Please download and install it from https://www.tenable.com/products/nessus\e[0m"
                fi
                ;;
            "Nikto")
                if command_exists nikto; then
                    echo -e "\e[33mRunning Nikto...\e[0m"
                    run_nikto
                else
                    install_package nikto
                    if command_exists nikto; then
                        echo -e "\e[33mRunning Nikto...\e[0m"
                        run_nikto
                    fi
                fi
                ;;
            "OpenSCAP")
                if command_exists oscap; then
                    echo -e "\e[33mRunning OpenSCAP...\e[0m"
                    run_openscap
                else
                    install_package openscap-utils
                    if command_exists oscap; then
                        echo -e "\e[33mRunning OpenSCAP...\e[0m"
                        run_openscap
                    fi
                fi
                ;;
            "Lynis")
                if command_exists lynis; then
                    echo -e "\e[33mRunning Lynis...\e[0m"
                    run_lynis
                else
                    install_package lynis
                    if command_exists lynis; then
                        echo -e "\e[33mRunning Lynis...\e[0m"
                        run_lynis
                    fi
                fi
                ;;
            "Metasploit")
                if command_exists msfconsole; then
                    echo -e "\e[33mRunning Metasploit...\e[0m"
                    run_metasploit
                else
                    echo -e "\e[31mMetasploit is not installed. Please follow the installation instructions at https://metasploit.help.rapid7.com/docs/using-the-metasploit-installers\e[0m"
                fi
                ;;
            "OWASP ZAP")
                if [ -f ./zap.sh ]; then
                    echo -e "\e[33mRunning OWASP ZAP...\e[0m"
                    run_owasp_zap
                else
                    echo -e "\e[31mOWASP ZAP is not installed or not found in the current directory. Please download and install it from https://www.zaproxy.org/download/\e[0m"
                fi
                ;;
            "Quit")
                break
                ;;
            *)
                echo -e "\e[31mInvalid option $REPLY\e[0m"
                ;;
        esac
    done
}

# Execute the main menu
main_menu
```
![Screenshot_2024-07-19_21_19_24](https://github.com/user-attachments/assets/66e07552-7204-4c52-804b-75decf560076)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request or open an Issue if you have any suggestions or bug reports.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

This README provides an overview of the script, including its features, installation steps, usage instructions, and script content. It also includes a section for contributing and the license under which the project is distributed.
