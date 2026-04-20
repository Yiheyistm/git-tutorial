# Project Deployment Report: Alternative Two - Practical Project

## 1. Introduction & Project Scope
This report documents the completion of **Alternative Two: Practical Project**, worth 20% of the course grade plus a 5-point bonus. The objective was to set up a virtualized environment on a major cloud platform, create virtual machines, and deploy a final year project to demonstrate full cloud-based functionality.

For this project, **Microsoft Azure** was selected as the cloud platform to host the **Admas Education System Hub**.

---

## 2. Phase 1: Setting up the Virtualized Environment
The environment was provisioned using Azure's native virtualization tools.

### Steps:
1. **Virtualization Tool**: Utilized **Azure Virtual Machines** and **Azure Resource Manager (ARM)** to provision resources.
2. **Compute Resource**: Created a VM instance running **Ubuntu 22.04 LTS**.
3. **Instance Sizing**: Allocated a "Standard_B1s" (or equivalent) instance, providing a balance of CPU and memory for the hub's requirements.
4. **Public Networking**: Assigned a static Public IP (`20.94.192.239`) to ensure persistent connectivity.
5. **Security**: Implemented **SSH Public Key (RSA 2048-bit)** authentication for secure administrative access.

---

## 3. Phase 2: Environment Configuration
Once the VM was provisioned, the server environment was prepared for the Node.js application.

### SSH Access:
Connected to the server via terminal:
```bash
ssh azureuser@20.94.192.239
```

### Installing Node.js & npm:
Since the application is built with React and Vite, a modern Node.js environment was required.
```bash
# Add NodeSource repository for Node.js 20.x
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Install Node.js and npm
sudo apt-get install -y nodejs
```

---

## 4. Phase 3: Application Setup
The application code was pulled from the repository and prepared for production.

### Cloning the Project:
```bash
git clone https://github.com/Yigoya/admas-education-system-hub-42.git
cd admas-education-system-hub-42
```

### Installation & Build:
1. **Install Dependencies**: Downloaded all required packages including React, Tailwind CSS, and Shadcn UI components.
   ```bash
   npm install
   ```
2. **Build Production Bundle**: Generated an optimized static site in the `dist/` directory.
   ```bash
   npm run build
   ```

---

## 5. Phase 4: Production Deployment with PM2
To ensure the application remains online 24/7 and handles traffic efficiently, it was deployed using **PM2** (Process Manager 2).

### Tools Installation:
```bash
sudo npm install -g pm2 serve
```

### PM2 Configuration:
An `ecosystem.config.cjs` file was created to manage the process:
```javascript
module.exports = {
  apps: [{
    name: 'admas-hub',
    script: 'serve',
    env: {
      PM2_SERVE_PATH: './dist',
      PM2_SERVE_PORT: 3000,
      PM2_SERVE_SPA: 'true'
    }
  }]
};
```

### Launching the App:
```bash
pm2 start ecosystem.config.cjs
```

---

## 6. Phase 5: Networking & Security (NSG)
By default, Azure blocks external traffic to non-standard ports. To make the application accessible, the Network Security Group (NSG) was modified.

### Inbound Rule Configuration:
- **Protocol**: TCP
- **Port**: 3000
- **Action**: Allow
- **Description**: Enabled traffic for the Admas Education System Hub.

---

## 7. Demonstration of Functionality
As per the project requirements, the application's functionality is demonstrated through its live deployment.

- **Dynamic Frontend**: The React-based interface is fully responsive and interactive.
- **Service Availability**: The hub is maintained in an "always-on" state by the PM2 process manager.
- **External Accessibility**: The system is accessible globally via the configured Azure endpoint.

## 8. Conclusion
By utilizing Azure's virtualization tools, the **Admas Education System Hub** has been successfully transitioned from a local development state to a professional cloud-based deployment. All steps, from environment provisioning to security configuration, have been documented to ensure reproducibility.

**Final Deployment Link**: [http://20.94.192.239:3000/](http://20.94.192.239:3000/)
