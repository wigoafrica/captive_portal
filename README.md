Here’s a clear breakdown of the process, focusing on **what Flutter does**, **what pfSense does**, and **how they relate**. This explanation is tailored so you can share it with others, including team members or stakeholders.

---

### **1. Overview**
The system combines two key technologies:
1. **pfSense**: A firewall and network management tool that handles the captive portal and access control.
2. **Flutter Web App**: A user-facing interface for login, signup, subscriptions, and payments.

The two components are linked through a **shared database** and a **backend API**, ensuring seamless communication.

---

### **2. What Each Component Does**

#### **A. pfSense Responsibilities**
pfSense is responsible for **network-level access control**:
- **Captive Portal**: Redirects users to the login page if they try to access the internet without authentication.
- **Authentication Enforcement**:
  - Determines if a user is allowed access based on their MAC address or subscription status.
  - Disconnects users when their subscription expires or they log out.
- **Redirection**: Sends unauthenticated users to the Flutter app (e.g., `http://yourapp.com/login`).
- **Bandwidth Control** (Optional): Limits internet speed or data usage for different subscription tiers.

#### **B. Flutter Web App Responsibilities**
Flutter handles all **user-facing functionality**:
- **User Interface**: Displays login, signup, and subscription pages in a modern, responsive design.
- **Authentication**:
  - Allows users to log in via email, Google, or phone.
  - Ensures only registered users with valid credentials can proceed.
- **Subscription Management**:
  - Enables users to view, purchase, or renew internet plans.
  - Processes payments (e.g., via Airtel Money or MTN Mobile Money).
- **Device Management**:
  - Tracks which devices (MAC addresses) are linked to a user’s account.
  - Allows users to see or update their active devices.
- **User Dashboard** (Optional): Displays account details, active devices, and subscription status.

#### **C. Backend API**
The **backend server** acts as the bridge between **Flutter** and **pfSense**:
- **Handles Authentication**:
  - Validates user credentials during login.
  - Verifies subscriptions and updates user access permissions in pfSense.
- **Updates the Database**:
  - Stores user data, subscription details, and connected devices.
  - Logs user actions for analytics or troubleshooting.
- **Communicates with pfSense**:
  - Provides pfSense with user authentication and subscription data.
  - Responds to pfSense queries about user access status.

---

### **3. How Flutter and pfSense Work Together**

#### **Step-by-Step Workflow**
1. **User Connects to the Wi-Fi**:
   - The user connects to your network (managed by pfSense).
   - If the user isn’t authenticated, pfSense redirects them to the Flutter web app (e.g., `http://yourapp.com/login`).

2. **User Logs In or Signs Up** (Flutter):
   - The user enters their credentials (or signs up if new).
   - Flutter sends this data to the backend for verification and storage.
   - If the login is successful, the backend updates the database and pfSense with the user’s access details (e.g., MAC address, subscription status).

3. **Subscription Check** (Flutter & Backend):
   - If the user has an active subscription, the backend informs pfSense to allow internet access for that user’s MAC address.
   - If not, the user is directed to the subscription page in the Flutter app to purchase a plan.

4. **pfSense Grants or Denies Access**:
   - pfSense checks the user’s status with the backend (or queries the database directly).
   - If valid, pfSense allows internet access for the user’s device.
   - If invalid, the user remains restricted and redirected to the Flutter app.

5. **User Manages Account** (Flutter):
   - The user can view or modify their subscription, add/remove devices, or check account details in the Flutter app.
   - Changes made in Flutter are synced to the database and updated in pfSense by the backend.

---

### **4. Communication Between Components**

| **Action**             | **Flutter**                       | **pfSense**                  | **Backend**                   |
|-------------------------|-----------------------------------|------------------------------|-------------------------------|
| User Login/Signup       | Displays login/signup form.       | Redirects to login page.     | Verifies credentials and updates database. |
| Subscription Purchase   | Displays subscription plans.      | -                            | Processes payments and updates user status. |
| Device Authentication   | Sends device details to backend. | Queries backend for MAC/IP.  | Validates and syncs device data with pfSense. |
| Access Control          | -                                | Allows/denies internet.      | Responds to pfSense with user permissions. |

---

### **5. Diagram of the Process**

Here’s a simplified diagram of how everything works:

```
[User Device] --> Connects to Wi-Fi --> [pfSense]
            ↘ Redirect to Login Page ↙
              [Flutter Web App]
                     ↕
         Sends Data to [Backend]
                     ↕
       Updates/Checks [Database]
                     ↕
        Informs [pfSense] of Status
                     ↕
[User Device] <-- Internet Access (if authenticated)
```

---

### **6. Advantages of This Setup**
- **pfSense** focuses on network-level control, ensuring stability and reliability.
- **Flutter** provides a modern, user-friendly interface for account management and subscriptions.
- The **backend** ensures seamless communication and synchronization between the two systems.
- The **shared database** centralizes user data, making it easy to scale and integrate new features.

