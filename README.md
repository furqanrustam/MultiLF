# MultiLF

# MEnDataset




# Phase 1: Data Collection

We construct a comprehensive dataset encompassing benign and malicious traffic from both traditional (non-IoT) and IoT network environments. To achieve this, we employ a two-part strategy.

**Part 1: DDoShield-IoT Testbed**
We utilize the DDoShield-IoT testbed ^1, which integrates Docker containers with NS-3 network simulations. This testbed simulates realistic network conditions by running real binaries within Docker containers over an emulated network topology, enabling the generation of authentic network traffic.

- Benign Traditional Traffic: FTP, HTTP, RTMP streaming

* Malicious IoT Traffic: Generated using Mirai malware binaries

**Part 2: External Dataset Integration**
Since DDoShield-IoT does not support benign IoT traffic or malicious traditional traffic, we integrate validated external PCAP datasets:

- Benign IoT Traffic: MQTT-IoT-IDS2020 dataset

* Malicious Traditional Traffic: CIC DDoS 2019 dataset

**Dataset Merging and Labeling**
To merge all traffic sources, we employ a Python script running inside a Docker container node (configured in promiscuous mode) within the DDoShield-IoT simulation. This script captures:

- Live traffic from DDoShield-IoT

* Randomly sampled packets from external PCAPs

+ All packets, regardless of their origin, are processed uniformly.

**Traffic Labeling**
- Traffic is labeled using known ground truths:

* DDoShield-IoT Traffic: Labeled based on simulation settings

+ External Datasets: Include validated labels from literature

To ensure comparability across traffic types, all packets undergo identical feature extraction procedures (detailed in Phase 2: Feature Extraction). This creates a uniform and unbiased dataset representing realistic M-En (Multi-Environment Network) conditions.


# Phase 2: Feature Extraction and Processing
After constructing a comprehensive M-En traffic dataset (described in Phase 1: Data Collection), we extract meaningful features that enable our autoencoder model to effectively identify network anomalies. We also apply necessary preprocessing steps, including normalization and splitting the dataset into training and testing subsets. Below, we first discuss Feature Extraction, followed by Data Processing.

**Feature Extraction**
Feature extraction is essential because raw network packets alone lack sufficient descriptive power to differentiate benign from malicious behaviors accurately. Therefore, we extract two complementary categories of features from the collected packets:

- General Features

* Statistical Features

**General Features**
These features capture fundamental properties of each individual packet, reflecting essential attributes of the network communication:

- Timestamp – Packet arrival time for chronological analysis

* Source & Destination IP Addresses – Helps trace traffic origin and destination

+ Protocol (TCP/UDP) – Differentiates protocol-specific traffic behavior

- Source & Destination Ports – Indicates application-level communication endpoints

* Packet Size & Payload Size – Describes packet structure and potential anomalous behavior

+ TCP Flags (SYN, ACK, FIN, PSH, URG, RST) – Used to detect scanning, flooding, and connection states

- Sequence & Acknowledgment Numbers – Useful in identifying session hijacking or manipulation

* TTL (Time to Live) – Useful for routing behavior analysis

While these features offer granular insights, they are insufficient alone for robust anomaly detection, especially for DDoS attacks which manifest as patterns over time. Hence, we augment them with statistical features aggregated over fixed intervals.

**Statistical Features**
These features capture temporal network behavior, aggregated over 1-second intervals:

- Packet Count – Total packets within the interval; high counts can indicate flooding

* Destination Port Entropy – Measures randomness in target ports, useful for identifying scanning

+ Most Frequent Source/Destination Ports – Highlights potentially targeted services

- Short-lived Connections – Rapid connection terminations, suggesting probes or scans

* Repeated Connection Attempts – Frequent retries may indicate brute-force attacks

+ Network Scanning Activity – SYN packets without ACK responses reveal incomplete handshakes

- Flow Rate – Packet throughput rate for traffic load analysis

- Source Entropy – Measures diversity of source IPs to detect distributed attacks

- Connection Errors (RST Flags) – Sudden terminations may signal malicious attempts

- Most Frequent & Abnormal Packet Size Frequency – Helps detect anomalies like uniform or oversized packets

- Sequence Number Variance – Irregularities hint at spoofing or session hijacking

- SYN & ACK Frequencies – Track connection establishment/response behavior

- TCP & UDP Frequencies – Monitors protocol usage imbalance

- Packet Size Variability, Average Packet Size, and Payload Sizes – Describe traffic diversity and structure

Data link: https://www.kaggle.com/datasets/furqanrustam118/men-dataset/

**Cite it** Rustam, F., Obaidat, I. and Jurcut, A.D., 2025. MULTI-LF: A Unified Continuous Learning Framework for Real-Time DDoS Detection in Multi-Environment Networks. arXiv preprint arXiv:2504.11575.

- 1. S. De Vivo, I. Obaidat, D. Dai, and P. Liguori, “DDoShield-IoT: A
testbed for simulating and lightweight detection of IoT botnet DDoS
attacks,” in Proceedings of the 54th Annual IEEE/IFIP International
Conference on Dependable Systems and Networks Workshops (DSN-W),
2024, pp. 1–8.


