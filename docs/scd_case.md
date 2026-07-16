# SCD-Derived Scheduling Case

The SCD-derived scheduling case combines a published IEC 61850 topology with
representative multi-vendor asset metadata and real ICS CVEs.

## SCD Inventory and Topology

The SCD-based scheduling case is built from the published rapid61850 SCD for
substation S12. The source SCD contains:

- two voltage levels: 220 kV D1 and 132 kV E1;
- four protection bays;
- twelve IEDs using IEC 61850 role names:
  - `SB` for system-bay primary protection;
  - `BP` for backup protection;
  - `KA` for bay-level RTUs.

A station-level SCADA server is added following the SCD-implied hierarchy. The
device list, bay assignments, voltage levels, and GOOSE/SV/MMS communication
topology are taken from the SCD.

Primary-backup mutual-exclusion pairs follow IED naming within each bay. The
SCADA-to-RTU MMS precedence relation follows the `ExtRef` bindings. The
resulting topology has:

- 13 cyber-relevant devices;
- 4 operational functions;
- 5 protection zones;
- 27 communication links.

## Asset-to-Role Mapping

IEC 61850 SCL files specify logical structure, communication bindings, and IED
hierarchy, but they do not generally encode complete vendor firmware
inventories or CVE-relevant version metadata. Therefore, representative
multi-vendor asset metadata is instantiated on top of the SCD-derived topology.

The inventory follows the IED roles in the SCD:

| SCD role | Asset metadata |
| --- | --- |
| `SB` primary protection IEDs | Siemens SIPROTEC 5 and ABB Relion 670 series, 5 devices |
| `BP` backup protection IEDs | ABB Relion 615 / 611 series, 4 devices |
| `KA` bay-level RTUs | Hitachi Energy RTU500 series, 3 devices |
| Station-level SCADA server | AVEVA InTouch HMI, 1 device |

## CVE Assignment

A subset of 15 real ICS CVEs relevant to this inventory is drawn from the
applicability corpus and matched against the 13-device inventory. This yields
195 CVE-device pairs with 14 positive applicable pairs.

The assigned firmware versions are real values placed inside or outside the
affected ranges of the matched advisories, so every applicability decision is
verifiable against public NVD/CISA sources.
