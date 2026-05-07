# WiFi Aggregation 
The objective of this page is to provide a detailed understanding of aggregation. This feature was introduced in 802.11n to address the inefficiency where the PHY header (fixed at 20µs) takes more time to transmit than the actual payload at high data rates.

> **Important:** Aggregation only occurs when **QoS (Quality of Service)** is enabled. It requires the use of QoS Data frames.

## 1. Aggregation Methods
1. **A-MSDU:** Aggregate MAC Service Data Unit
2. **A-MPDU:** Aggregate MAC Protocol Data Unit
3. **Two-Level Aggregation:** Both A-MSDU and A-MPDU used together.

| Feature | A-MSDU | A-MPDU |
| :--- | :--- | :--- |
| **Full Name** | Aggregate MAC Service Data Unit | Aggregate MAC Protocol Data Unit |
| **Error Recovery** | All-or-nothing (One FCS) | Selective Retransmission (Multiple FCS) |
| **OSI Layer** | Upper MAC | Lower MAC |
| **Efficiency** | Highest (Least overhead) | High (Balanced) |


MPDU DELIMITER is added to understand where a AMPDU subframe starts/ends

if we sniff, we cannot see AMPDUs as they will be processed. 

payload type : AMSDU or MSDU
radio information element-> Last part of AMPDU set to 1. 

the max airtime of PPDU is 5.484 ms


## 2. Aggregation Flow Diagram

```mermaid
graph TD
    subgraph MSDU_Level [1. MSDU Level]
    A["MSDU Frame<br/>(DA | SA | Length | Payload | Padding)"]
    end

    subgraph AMSDU_Level [2. A-MSDU Level]
    B1[AMSDU sub 1] --- B2[AMSDU sub 2] --- B3[...] --- BN[AMSDU sub N]
    end

    subgraph MAC_Level [3. MPDU Level]
    C["MAC Header | A-MSDU | FCS"]
    end

    subgraph MPDU_Detail [4. Subframe Structure]
    D["MPDU Delimiter | MPDU | Padding"]
    end

    subgraph AMPDU_Level [5. A-MPDU Level]
    E1[AMPDU sub 1] --- E2[AMPDU sub 2] --- E3[...] --- EN[AMPDU sub N]
    end

    subgraph PHY_Level [6. PPDU Level]
    F["PHY Header | PSDU (AMPDU)"]
    end

    A --> B1
    BN --> C
    C --> D
    D --> E1
    EN --> F

    style F fill:#f9f,stroke:#333,stroke-width:2px
    style A fill:#bbf,stroke:#333



