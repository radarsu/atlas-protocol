### **Group 1: Local-Only Apps**
*Not communicating with Storages.*

These are privacy-first, standalone tools. They function like traditional "offline" software but within a modern web/mobile framework.

### **Group 2: User-Sovereign Apps**
*Directly communicating with Storages, but all processing logic is client-side.*

The app is just a "viewer" or "editor”. The "brain" of the app runs on **your device** (Client-Side). The app fetches data, performs the logic (like sorting your social feed or calculating a budget), and then saves the result back to your storage.

### Group 3: Delegated Logic (Compute-Over-Data)
*Communicating with Storage, but processing logic happens on a remote (but verifiable) server.*

In this group, the data is still yours, but the "math" is too heavy for your phone (e.g., training a private AI model or indexing millions of decentralized posts). Apps uses transparent, known and generally verifiable algorithms.

### Group 4: Platform-Locked Apps (The Legacy Model)
*Enshittification Zone*

Communicating with Centralized/Siloed Storages. All logic is server-side and proprietary. This is the current state of **Facebook, X (Twitter), and Instagram.** In this group, the platform owns the "Data," the "Logic," and the "Network."

### Summarization of Group Hierarchies

1. **Local-Only:** Privacy-first, zero-leakage tools.
2. **User-Sovereign:** You own the data; apps are just "skins."
3. **Delegated Logic:** Remote power (AI/Search) that is mathematically verifiable.
4. **Platform-Locked:** The "Legacy" silo where data is a hostage (The Enshittified State).

| Group | Data Ownership | Logic Execution | Connection Type | Philosophy |
| :--- | :--- | :--- | :--- | :--- |
| **1. Local-Only** | Device Storage | Local (Client) | Air-gapped / Offline | Total isolation. |
| **2. User-Sovereign** | User's Trusted Node | Local (Client) | Direct Peer-to-Peer | Data is a utility; apps are just skins. |
| **3. Delegated Logic** | User's Trusted Node | Remote (Verifiable) | Compute-over-Data | Power of a server, privacy of a local app. |
| **4. Platform-Locked** | Service Provider | Remote (Proprietary) | Siloed API | The Enshittification Zone. |