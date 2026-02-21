# Atlas - Types of Apps

## Group 1: Local-Only Apps

*Not communicating with storage nodes.*

These are privacy-first, standalone tools. They behave like traditional offline software inside modern web/mobile frameworks.

## Group 2: User-Sovereign Apps

*Directly communicating with storage nodes, with all processing logic on the client side.*

The app is just a viewer/editor. The "brain" runs on **your device**. The app fetches data, performs logic (for example sorting a social feed or calculating a budget), and saves results back to your storage.

## Group 3: Delegated Logic (Compute-over-Data)

*Communicating with storage, but processing logic runs on a remote (verifiable) server.*

Data is still user-owned, but the computation is too heavy for a device (indexing millions of decentralized posts into algorithms, more non-standard features). Apps use transparent, known, and generally verifiable algorithms.

## Group 4: Platform-Locked Apps (Legacy Model)

*Enshittification zone.*

These apps communicate with centralized/siloed storage. All logic is proprietary and server-side. This is the current state of platforms like **Facebook, X (Twitter), and Instagram**. The platform owns the data, logic, and network.

## Summary

1. **Local-Only:** Privacy-first, zero-leakage tools.
2. **User-Sovereign:** You own the data; apps are just skins.
3. **Delegated Logic:** Remote power (AI/search) with verifiable computation.
4. **Platform-Locked:** Legacy silo model where user data is hostage.

| Group | Data Ownership | Logic Execution | Connection Type | Philosophy |
| :--- | :--- | :--- | :--- | :--- |
| **1. Local-Only** | Device Storage | Local (Client) | Air-gapped / Offline | Total isolation. |
| **2. User-Sovereign** | User's Trusted Node | Local (Client) | Direct Peer-to-Peer | Data is a utility; apps are skins. |
| **3. Delegated Logic** | User's Trusted Node | Remote (Verifiable) | Compute-over-Data | Server power, local-app privacy. |
| **4. Platform-Locked** | Service Provider | Remote (Proprietary) | Siloed API | Enshittification zone. |
